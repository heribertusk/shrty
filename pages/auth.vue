<script lang="ts" setup>
import { AuthError } from "@supabase/gotrue-js";

definePageMeta({
  middleware: "guest",
});

// const config = useRuntimeConfig();
const supabaseAuth = useSupabaseAuthClient();

useHead({
  title: "Authenticate",
});

const form = ref<{
  type: "login" | "register";
  email: string;
  password: string;
}>({
  type: "login",
  email: "",
  password: "",
});

const err = ref<AuthError | null>(null);

// const isLoading = ref(false);
// const handleGithubLogin = async () => {
//   const { auth } = useSupabaseAuthClient();
//   try {
//     isLoading.value = true;
//     auth.signInWithOAuth({
//       provider: "github",
//       options: {
//         redirectTo: "http://localhost:3000",
//       },
//     });
//   } catch (error) {
//     console.log(error);
//   } finally {
//     isLoading.value = false;
//   }
// };
const handleAuthSubmit = async (e: Event) => {
  // if (form.value.type === "login") return handleLogin();
  //
  // return handleRegister();
  return handleLogin();
};

const handleLogin = async () => {
  const { data, error } = await supabaseAuth.auth.signInWithPassword({
    email: form.value.email,
    password: form.value.password,
  });

  if (error) {
    err.value = error;
    console.log(error);
  }

  if (data) {
    console.log(data);

    // Redirect to home page
    await navigateTo("/dashboard");
  }
};

// const handleRegister = async () => {
//   const { data, error } = await supabaseAuth.auth.signUp({
//     email: form.value.email,
//     password: form.value.password,
//   });
//
//   if (error) {
//     err.value = error;
//     console.log(error);
//   }
//
//   if (data) {
//     console.log(data);
//   }
// };
</script>
<template>
  <div class="grid place-content-center h-screen auth">
    <div class="container mx-auto">
      <div v-if="err && err.message" class="mb-5 text-center card-error">
        {{ err.message }}
      </div>
      <div class="p-5 rounded-2xl border card bg-white/5 backdrop-blur-lg border-white/10 w-[300px]">
        <div class="flex justify-center text-center">
          <div class="p-4 rounded-full border border-white/10">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="w-8 h-8">
              <path stroke-linecap="round" stroke-linejoin="round"
                d="M16.5 10.5V6.75a4.5 4.5 0 10-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 002.25-2.25v-6.75a2.25 2.25 0 00-2.25-2.25H6.75a2.25 2.25 0 00-2.25 2.25v6.75a2.25 2.25 0 002.25 2.25z" />
            </svg>
          </div>
        </div>
        <!-- <button class="mt-5 w-full btn" @click="handleGithubLogin" :disabled="isLoading"> -->
        <!--   Continue with github -->
        <!-- </button> -->
        <!---->
        <hr class="my-5 border border-white/10" />

        <form action="" @submit.prevent="handleAuthSubmit">
          <div class="form-group">
            <label for="email">Email</label>
            <input id="email" v-model="form.email" type="text" class="" placeholder="john@gmail.dev" />
          </div>
          <div class="mt-2 form-group">
            <label for="password">Password</label>
            <input id="password" v-model="form.password" type="password" class="" />
          </div>
          <button class="my-5 w-full text-sm btn" type="submit">
            <!-- <template v-if="form.type == 'login'">Login</template> -->
            <!-- <template v-else>Register</template> -->
            Login
          </button>
        </form>
        <!-- <div class="text-center"> -->
        <!--   <button class="text-white/50" @click="form.type = form.type === 'register' ? 'login' : 'register'"> -->
        <!--     <template v-if="form.type == 'login'">Dont have an account? Register</template> -->
        <!--     <template v-else>Already have an account? Login</template> -->
        <!--   </button> -->
        <!-- </div> -->
      </div>
    </div>
  </div>
</template>
