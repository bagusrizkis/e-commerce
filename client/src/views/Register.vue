<template>
    <v-container grid-list-xs>
        <v-card width="500px" class="mx-auto mt-5">
            <v-card-title primary-title>
                Sign up to unShop
                <v-spacer></v-spacer>
            </v-card-title>
            <v-container>
                <v-form>
                    <v-text-field
                        v-model="email"
                        name="email"
                        label="Email"
                        :rules="emailRules"
                        type="email"
                        prepend-icon="mdi-email"
                        required
                    ></v-text-field>

                    <v-text-field
                        v-model="password"
                        name="password"
                        label="Password"
                        :type="showPassword ? 'text' : 'password'"
                        prepend-icon="mdi-lock"
                        :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                        @click:append="showPassword = !showPassword"
                        required
                        :rules="[rules.required]"
                    ></v-text-field>
                    <v-divider></v-divider>

                    <v-alert v-if="formHasErrors" dense outlined type="error">
                        <strong>Email</strong> already Registered
                    </v-alert>

                    <v-card-actions>
                        <v-btn
                            class="mt-5"
                            color="primary"
                            @click="submit"
                            :loading="loading"
                            :disabled="email == '' || password == ''"
                        >
                            Register
                        </v-btn>
                        <v-spacer></v-spacer>
                        <v-btn
                            class="mt-5"
                            @click="goToLogin"
                        >
                            I have account
                        </v-btn>
                    </v-card-actions>
                </v-form>
            </v-container>
        </v-card>
    </v-container>
</template>

<script>
export default {
  name: 'Register',
  data () {
    return {
      loading: false,
      showPassword: false,
      email: '',
      password: '',
      emailRules: [
        v => !!v || 'E-mail tidak boleh kosong',
        v => /.+@.+\..+/.test(v) || 'E-mail harus valid'
      ],
      rules: {
        required: value => !!value || 'Password Tidak Boleh kosong.',
        emailMatch: () => 'Email atau password yang dimasukkan salah'
      },
      formHasErrors: false
    }
  },
  methods: {
    submit () {
      // return this.$router.push('/dash')
      this.formHasErrors = false
      this.loading = !this.loading
      return this.$store
        .dispatch('register', {
          email: this.email,
          password: this.password
        })
        .then(result => {
          console.log('dari methods', result)
          this.loading = !this.loading
          if (result === undefined) {
            this.$router.push('/')
          } else {
            this.formHasErrors = true
            console.log('masuk')
          }
        })
    },
    goToLogin () {
      this.$router.push('/login')
    }
  },
  computed: {
    emailErrors () {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.email && errors.push('Must be valid e-mail')
      !this.$v.email.required && errors.push('E-mail is required')
      return errors
    }
  }
}
</script>

<style></style>>
