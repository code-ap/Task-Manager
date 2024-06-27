<template>
  <div class="sm:mx-auto sm:w-full sm:max-w-md bg-white p-8 shadow-md rounded-md">
    <div class="u-bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10 flex items-center justify-center">
      <div>
        <div class="relative">
          <div class="absolute inset-0 flex items-center">
            <div class="w-full border-t border-gray-300" />
          </div>
          <div class="relative flex justify-center text-sm">
            <span class="px-2 u-bg-white text-gray-500"> Connect with </span>
          </div>
        </div>
        <slot />
      </div>
    </div>
  </div>
</template>
