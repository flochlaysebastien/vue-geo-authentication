<template>
  <div class="signup container">
    <form class="card-panel" @submit.prevent="signup">
      <h2 class="center deep-purple-text">Signup</h2>
      <div class="field">
        <label for="email">Email</label>
        <input id="email" type="email" v-model="email" />
      </div>
      <div class="field">
        <label for="password">Password</label>
        <input id="password" type="password" v-model="password" />
      </div>
      <div class="field">
        <label for="name">Alias</label>
        <input id="name" type="text" v-model="alias" />
      </div>
      <p class="red-text center" v-if="feedback">{{ feedback }}</p>
      <div class="field center">
        <button class="btn deep-purple">Signup</button>
      </div>
    </form>
  </div>
</template>

<script>
import db from "@/firebase/init";
import slugify from "slugify";

export default {
  name: "Signup",
  data() {
    return {
      email: null,
      password: null,
      alias: null,
      feedback: null,
      slug: null
    };
  },
  methods: {
    signup() {
      if (!this.alias) {
        this.feedback = "Please enter an alias";
        return;
      }

      this.feedback = null;
      this.slug = slugify(this.alias, {
        replacement: "-",
        remove: /[$*_+~.()'"!\-:@]/g,
        lower: true
      });

      let ref = db.collection("users").doc(this.slug);
      ref.get().then(doc => {
        if (doc.exists) {
          this.feedback = "This alias already exists";
        }
      });
      console.log(this.slug);
    }
  }
};
</script>

<style>
.signup {
  max-width: 400px;
  margin-top: 60px;
}
.signup h2 {
  font-size: 2.4em;
}
.signup .field {
  margin-bottom: 16px;
}
</style>