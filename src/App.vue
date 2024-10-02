<template>
  <div>
    <header class="bg-[#1c1e53]">
      <div class="container mx-auto py-5 flex justify-between items-center px-32">
        <img src="./img/Logo.svg" alt="logo" />
        <ul class="flex gap-12 items-center">
          <li><a href="" class="text-white text-base font-medium">Home</a></li>
          <li><a href="" class="text-white text-base font-medium">About Us</a></li>
          <li><a href="" class="text-white text-base font-medium">Pricing</a></li>
          <li><a href="" class="text-white text-base font-medium">Work</a></li>
          <li><a href="" class="text-white text-base font-medium">Blog</a></li>
          <button
            class="bg-transparent border border-white rounded-full px-8 py-3 text-white text-base font-medium"
          >
            Contact Us
          </button>
        </ul>
      </div>
    </header>

    <main class="px-32 text-center py-[150px]">
      <h2 class="font-[600] text-[48px] text-[#282938]">Contact Us</h2>
      <p class="font-[400] text-[16px text-[#282938] mb-10">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do <br />
        eiusmod tempor incididunt ut labore.
      </p>

      <form @submit.prevent="submitForm" class="bg-[#F4F6FC] w-full max-w-[800px] mx-auto p-8 rounded-lg shadow-lg">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700 mb-1 text-start">Name</label>
            <input v-model="name" type="text" name="name" id="name" class="w-full bg-transparent border border-gray-300 rounded-md py-2 px-4 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Enter your name" required />
          </div>
          <div>
            <label for="email" class="block text-sm font-medium text-gray-700 mb-1 text-start">Email</label>
            <input v-model="email" type="email" name="email" id="email" class="w-full bg-transparent border border-gray-300 rounded-md py-2 px-4 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Enter your email" required />
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
          <div>
            <label for="subject" class="text-start block text-sm font-medium text-gray-700 mb-1">Subject</label>
            <input v-model="subject" type="text" name="subject" id="subject" class="w-full border bg-transparent border-gray-300 rounded-md py-2 px-4 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Provide context" required />
          </div>
          <div>
            <label for="subject2" class="text-start block text-sm font-medium text-gray-700 mb-1">Subject</label>
            <input v-model="subject2" type="text" name="subject2" id="subject2" class="w-full border bg-transparent border-gray-300 rounded-md py-2 px-4 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Select subject" required />
          </div>
        </div>

        <div class="mb-6">
          <label for="message" class="text-start block text-sm font-medium text-gray-700 mb-1">Message</label>
          <textarea v-model="message" class="w-full border bg-transparent border-gray-300 rounded-md py-2 px-4 h-32 focus:outline-none focus:ring-2 focus:ring-blue-500 resize-none" name="message" id="message" placeholder="Write your question here" required></textarea>
        </div>

        <button 
          type="submit" 
          class="bg-[#282938] text-white font-bold py-4 px-12 rounded-[40px] " 
          :class="{ 'opacity-50': !canSubmit }" 
          :disabled="!canSubmit"
        >
          Send Message
        </button>

        <!-- Display success message -->
        <p v-show="successMessage" class="mt-4 text-green-500">{{ successMessage }}</p>
      </form>
    </main>

    <footer class="bg-[#1c1e53] py-14">
      <div class="container mx-auto flex justify-between px-32">
        <div>
          <img src="./img/Logo.svg" alt="logo" />
          <p class="text-white font-medium text-base mt-5">We are always open to discuss your project and <br />improve your online presence.</p>
          <div class="bg-[#fcd980] mt-10 pr-40 p-5 flex gap-12">
            <div>
              <p class="font-bold text-black">Email me at</p>
              <p class="text-black text-lg">contact@website.com</p>
            </div>
            <div>
              <p class="font-bold text-black">Call us</p>
              <p class="text-black text-lg">0927 6277 28525</p>
            </div>
          </div>
        </div>
        <div>
          <h2 class="text-white font-semibold text-4xl mb-5">Let's Talk!</h2>
          <p class="text-white font-light text-base mb-5">We are always open to discuss your project,<br /> improve your online presence and help with your <br /> UX/UI design challenges.</p>
          <img src="./img/Social_Media_Icons.svg" alt="sc_icons" />
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      name: '',
      email: '',
      subject: '',
      subject2: '',
      message: '',
      successMessage: '', 
      canSubmit: true 
    };
  },
  methods: {
    async submitForm() {
      const formData = {
        name: this.name,
        email: this.email,
        subject: this.subject,
        subject2: this.subject2,
        message: this.message
      };

      try {
        const response = await fetch('http://localhost:3001/contacts', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(formData)
        });

        if (!response.ok) {
          throw new Error('Network error');
        }

        const result = await response.json();
        console.log('Success:', result);

        this.successMessage = 'Message sent successfully!';

        this.name = '';
        this.email = '';
        this.subject = '';
        this.subject2 = '';
        this.message = '';
      } catch (error) {
        console.error('Error:', error);
      }
    }
  },
  watch: {
    name(val) {
      this.canSubmit = !!(val && this.email && this.subject && this.message);
    },
    email(val) {
      this.canSubmit = !!(this.name && val && this.subject && this.message);
    },
    subject(val) {
      this.canSubmit = !!(this.name && this.email && val && this.message);
    },
    message(val) {
      this.canSubmit = !!(this.name && this.email && this.subject && val);
    }
  },
  created() {
    console.log('Component created.');
  },
  mounted() {
    console.log('Component mounted');
  }
}
</script>
