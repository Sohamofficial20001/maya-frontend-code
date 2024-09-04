<!-- <template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <img src="@/assets/logo.svg" alt="Maya Logo" class="logo" />
          Maya</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div v-for="(message, index) in messages" :key="index" :class="message.role">
        <p>{{ message.content }}</p>
      </div>
    </ion-content>

    <ion-footer>
      <ion-toolbar>
        <ion-item lines="none">
          <ion-input
            v-model="message"
            placeholder="Type your query"
            @keyup.enter="sendMessage"
            clear-input
          ></ion-input>
          <ion-button @click="sendMessage">Send</ion-button>
        </ion-item>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script>
import { CapacitorHttp } from '@capacitor/core'; // Import CapacitorHttp
import { IonButton, IonFooter, IonInput, IonPage, IonTitle, IonImg } from '@ionic/vue';

export default {
  components: {
    IonButton, IonFooter, IonInput, IonPage, IonTitle, IonImg

  },
  data() {
    return {
       messages: [],
       message: '', // User input message
    };
  },
  methods: {
  //    async sendMessage() {
  //     const question = this.message.trim(); // Trim whitespace from input
  //     console.log('User Message:', question); // Debugging log to check the input value

  //     // // Check if the input is empty
  //      if (!question) {
  //        this.messages.push({ role: 'assistant', content: 'Question is empty.' });
  //        return;
  //      }

  //     // Add the user's message to the chat
  //     this.messages.push({ role: 'user', content: question });

  //    // Clear the input after sending the message
  //     this.message = '';

  //     const filename = sessionStorage.getItem('filename');

  //     try {
  //       // Send the input to the backend API
  //       const response = await CapacitorHttp.post({
  //         url: 'http://127.0.0.1:5000/api/generate-response',
  //         headers: { 'Content-Type': 'application/json' },
  //         data: { question, filename }, // Sending question as payload
  //       });
        
  //       // Check the response from the backend
  //       console.log('Response:', response); // Log the response to check its format
  //       const data = response.data;

  //       // Add the response from the backend to the chat
  //       this.messages.push({ role: 'assistant', content: data.response || 'No response received.' });
  //     } catch (error) {
  //       console.log('Error sending request:', error); // Log any errors encountered
  //       this.messages.push({ role: 'assistant', content: `Error: ${error.message}` });
  //     }


  //   },
  // },
  async sendMessage() {
    const question = this.message.trim();
    console.log('User Message:', question);

    if (!question) {
      this.messages.push({ role: 'assistant', content: 'Question is empty.' });
      return;
    }

    this.messages.push({ role: 'user', content: question });
    this.message = '';

    const filename = sessionStorage.getItem('filename');

    try {
      const response = await CapacitorHttp.post({
        url: 'http://localhost:5000/api/generate-response',
        headers: { 'Content-Type': 'application/json' },
        data: { question, filename },
      });

      const data = response.data;
      console.log('Response:', data);

      if (data.greet) {
        this.messages.push({ role: 'assistant', content: data.greet });
      }
      
      if (data.columns && data.data) {
        const formattedData = this.formatData(data.columns, data.data);
        this.messages.push({ role: 'assistant', content: formattedData });
      }

    } catch (error) {
      console.log('Error sending request:', error);
      this.messages.push({ role: 'assistant', content: `Error: ${error.message}` });
    }
  },

  formatData(columns, data) {
    let result = columns.join(', ') + '\n';
    data.forEach(row => {
      result += row.join(', ') + '\n';
    });
    return result;
  }
}
};
</script>

<style scoped>
.user {
  text-align: right;
  margin: 10px;
}

.assistant {
  text-align: left;
  margin: 10px;
}

/* Make sure the content doesn't overlap with the footer */
ion-content {
  --padding-bottom: 70px;
}

/* Ensure the footer is always at the bottom */
ion-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}

/* Logo styles */
.logo {
  height: 30px; 
  margin-right: 8px;
  vertical-align: middle; 
}

</style>
 -->


 <template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <img src="@/assets/logo.svg" alt="Maya Logo" class="logo" />
          Maya
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div v-for="(message, index) in messages" :key="index" :class="message.role">
        <template v-if="message.tableData">
          <table class="response-table">
            <thead>
              <tr>
                <th v-for="column in message.tableData.columns" :key="column">{{ column }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(row, rowIndex) in message.tableData.rows" :key="rowIndex">
                <td v-for="(cell, cellIndex) in row" :key="cellIndex">{{ cell }}</td>
              </tr>
            </tbody>
          </table>
        </template>
        <template v-else>
          <p>{{ message.content }}</p>
        </template>
      </div>
    </ion-content>

    <ion-footer>
      <ion-toolbar>
        <ion-item lines="none">
          <ion-input
            v-model="message"
            placeholder="Type your query"
            @keyup.enter="sendMessage"
            clear-input
          ></ion-input>
          <ion-icon :src = "sendOutline" class = "send-icon" @click="sendMessage"></ion-icon>
          <!-- <ion-button @click="sendMessage">Send</ion-button> -->
        </ion-item>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script>
import { CapacitorHttp } from '@capacitor/core'; 
import { IonButton, IonIcon, IonFooter, IonInput, IonPage, IonTitle, IonImg } from '@ionic/vue';
import { sendOutline } from 'ionicons/icons';

export default {
  components: {
    IonButton, IonFooter, IonInput, IonPage, IonTitle, IonImg, IonIcon
  },setup(){
    return{
      sendOutline
    }
  },
  data() {
    return {
      messages: [],
      message: '', 
    };
  },
  methods: {
    async sendMessage() {
      const question = this.message.trim();
      console.log('User Message:', question);

      if (!question) {
        this.messages.push({ role: 'assistant', content: 'Question is empty.' });
        return;
      }

      this.messages.push({ role: 'user', content: question });
      this.message = '';

      const filename = sessionStorage.getItem('filename');

      try {
        const response = await CapacitorHttp.post({
          url: 'http://127.0.0.1:5000/api/generate-response',
          headers: { 'Content-Type': 'application/json' },
          data: { question, filename },
        });

        const data = response.data;
        console.log('Response:', data);

        if (data.greet) {
          this.messages.push({ role: 'assistant', content: data.greet });
        }

        if (data.columns && data.data) {
          this.messages.push({ role: 'assistant', content: '', tableData: { columns: data.columns, rows: data.data } });
        }

      } catch (error) {
        console.log('Error sending request:', error);
        this.messages.push({ role: 'assistant', content: `Error: ${error.message}` });
      }
    }
  }
};
</script>

<style scoped>
.user {
  text-align: right;
  margin: 10px;
}

.assistant {
  text-align: left;
  margin: 10px;
}

/* Make sure the content doesn't overlap with the footer */
ion-content {
  --padding-bottom: 70px;
}

/* Ensure the footer is always at the bottom */
ion-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}

/* Logo styles */
.logo {
  height: 30px; 
  margin-right: 8px;
  vertical-align: middle; 
}

/* Table styles */
.response-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.response-table th,
.response-table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

.response-table th {
  background-color: #f8f8f8;
}
.send-icon {
  font-size: 24px;
  cursor: pointer;
  color: #3880ff; /* Adjust color as needed */
}
</style>

