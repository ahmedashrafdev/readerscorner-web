<template>
    <div class="login" :class="{active : $route.query.tab == 'login'}">
      <div v-if="loginReuired">
          <span>Please login to be able to continue checkout</span>
      </div>
        <div class="header">sign in</div>
        <div v-if="errors && typeof errors == 'string'">
          <span class="text-danger">{{errors}}</span>
        </div>
        <div v-if="errors && typeof errors == 'array'">
          <span v-for="(err , index) in errors" :key="index">{{err}}</span>
        </div>
        <v-form v-model="valid" ref="loginForm">
            <v-text-field
                label="Email"
                :rules="rules.email"
                v-model="form.email"
                ></v-text-field>
            <v-text-field
                label="Password"
                type="password"
                :rules="rules.password"
                v-model="form.password">
            </v-text-field>
            <v-btn class="btn" :loading="loading"  @click.prevent="login">
                <span>login</span>
            </v-btn>
        </v-form>
        <div class="disabled">
          <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Praesentium quam voluptatem numquam similique commodi vel,</p>
          <nuxt-link :to="{name : 'login' , query :{tab:'login'}}" class="btn-square block">click here to login</nuxt-link>
        </div>
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
  export default {
    methods:{
      login(){
        if(!this.loading){
          this.$refs.loginForm.validate();
          if(this.valid){
            this.form.ip = localStorage.getItem('ip')
            this.$store.dispatch('myAuth/login' , {"auth" : this.$auth ,'form' :this.form})
          }
        }
      }
    },
    data: () => ({
      valid:false,
      rules:{

        password: [
          v => !!v || 'password is required',
          v => v.length >= 6 || 'password must be more than than 6 characters',
        ],
        email: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
      },
      
      form:{
        email : 'ahmed@readerscorner.co',
        password : '123456',
      },
     
    }),
    computed: {
      ...mapGetters({
          loading: 'myAuth/loading',
          loginReuired: 'ui/loginRequired',
          errors: 'ui/formErrors'
      })
    },
  }
</script>

 <style  scoped src="@/assets/scss/components/login.css"></style>
