<template>
    <div class="flex flex-col items-center justify-center min-h-screen space-y-4">
      <!-- Email Sign In -->
      <div>
        <input v-model="email" type="email" placeholder="Enter your email" class="p-2 border rounded-md">
        <button @click="signInWithOtp" class="p-2 bg-blue-500 text-white rounded-md">Sign In with E-Mail</button>
      </div>
      <!-- GitHub OAuth Sign In -->
      <button @click="signInWithGitHub" class="p-2 bg-black text-white rounded-md">
        <i class="mdi-github"></i> Sign In with GitHub
      </button>
    </div>
  </template>

  <script setup lang="ts">
  const supabase = useSupabaseClient();
  const email = ref('');

  const signInWithOtp = async () => {
    const { error } = await supabase.auth.signInWithOtp({
      email: email.value,
      options: {
        emailRedirectTo: 'http://localhost:3000/confirm',
      },
    });
    if (error) console.error(error);
  };

  const signInWithGitHub = async () => {
    const { error } = await supabase.auth.signInWithOAuth({
      provider: 'github',
      options: {
        redirectTo: 'http://localhost:3000/confirm',
      },
    });
    if (error) console.error(error);
  };
  </script>
