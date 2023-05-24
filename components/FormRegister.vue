<template>
  <main class="min-h-screen bg-[#1a1a1a]" @submit.prevent="submitForm">
    <section class="container mx-auto px-5 py-14 text-gray-400">
      <form ref="form"
          class="bg-opacity-50  mx-auto flex w-full flex-col rounded-lg bg-[#242424] p-8 shadow-lg md:mt-0 md:w-1/2 lg:w-2/6"
      >
        <h2 class="mb-5 text-lg font-medium text-[#aac8e4]">Register</h2>

        <!-- Email input -->
        <div class="relative mb-5">
          <label for="email" class="text-sm leading-7 text-gray-400">Email</label>
          <div class="relative">
            <input
                v-model="formData.email"
                type="text"
                id="email"
                name="email"
                placeholder="e.g. example@email.com"
                class="bg-opacity-20 w-full rounded border border-gray-600 bg-transparent py-1 px-3 text-base leading-8 text-gray-100 outline-none transition-colors duration-200 ease-in-out placeholder:text-gray-500 focus:border-blue-500 focus:bg-transparent focus:ring-2 focus:ring-transparent"
                @change="v$.email.$touch()"
                :class="{'border-red-500 focus:border-red-500' : v$.email.$error,
                'border-[#42d392] ': !v$.email.$invalid
                }"
            />
            <Icon v-if="!v$.email.$invalid || v$.email.$error" class="absolute right-2 h-full text-xl text-green-500" :class="{ 'text-green-500': !v$.email.$invalid, 'text-yellow-500': v$.email.$error }"
                  :name="`heroicons-solid:${!v$.email.$error ? 'check-circle' : 'exclamation'}`"
            />
<!--            <span class="absolute left-0 -bottom-5 text-[12px] text-red-500">{{v$.email.$error}}</span>-->
          </div>
        </div>

        <!-- Password input -->
        <div class="relative mb-4">
          <label for="password" class="text-sm leading-7 text-gray-400">Password</label>
          <div class="relative">
            <input
                v-model="formData.password"
                type="password"
                id="password"
                name="password"
                class="bg-opacity-20 w-full rounded border border-gray-600 bg-transparent py-1 px-3 text-base leading-8 text-gray-100 outline-none transition-colors duration-200 ease-in-out focus:border-blue-500 focus:bg-transparent focus:ring-2 focus:ring-transparent"
                @change="v$.password.$touch()"
                :class="{'border-red-500 focus:border-red-500' : v$.password.$error,
                'border-[#42d392] ': !v$.password.$invalid
                }"
            />
            <Icon v-if="!v$.password.$invalid || v$.password.$error" class="absolute right-2 h-full text-xl text-green-500" :class="{ 'text-green-500': !v$.password.$invalid, 'text-yellow-500': v$.password.$error }"
                  :name="`heroicons-solid:${!v$.password.$error ? 'check-circle' : 'exclamation'}`"
            />
          </div>
        </div>

        <!-- Confirm Password input -->
        <div class="relative mb-4">
          <label for="confirmPassword" class="text-sm leading-7 text-gray-400"
          >Confirm password</label
          >
          <div class="relative">
            <input
                v-model="formData.confirmPassword"
                type="password"
                id="confirmPassword"
                name="confirmPassword"
                class="bg-opacity-20 w-full rounded border border-gray-600 bg-transparent py-1 px-3 text-base leading-8 text-gray-100 outline-none transition-colors duration-200 ease-in-out focus:border-blue-500 focus:bg-transparent focus:ring-2 focus:ring-transparent"
                @change="v$.confirmPassword.$touch()"
                :class="{'border-red-500 focus:border-red-500':v$.confirmPassword.$error,
                'border-[#42d392]': !v$.confirmPassword.$invalid}"
            />
            <Icon v-if="!v$.confirmPassword.$invalid || v$.confirmPassword.$error" class="absolute right-2 h-full text-xl text-green-500" :class="{ 'text-green-500': !v$.confirmPassword.$invalid, 'text-yellow-500': v$.confirmPassword.$error }"
                  :name="`heroicons-solid:${!v$.confirmPassword.$error ? 'check-circle' : 'exclamation'}`"
            />
          </div>
        </div>

        <!-- Code button -->
        <div class="relative mb-4">
          <label for="code" class="text-sm leading-7 text-gray-400"
          >Enter your code</label
          >
          <div class="relative">
            <input
                v-model="formData.code"
                type="number"
                id="code"
                name="code"
                class="bg-opacity-20 w-full rounded border border-gray-600 bg-transparent py-1 px-3 text-base leading-8 text-gray-100 outline-none transition-colors duration-200 ease-in-out focus:border-blue-500 focus:bg-transparent focus:ring-2 focus:ring-transparent"
                @change="v$.code.$touch()"
                :class="{'border-red-500 focus:border-red-500': v$.code.$error,
                'border-[#42d392]': !v$.code.$invalid}"
            />
            <Icon v-if="v$.code.$error || !v$.code.$invalid" class="absolute right-2 h-full text-xl text-green-500" :class="{'text-green-500': !v$.code.$invalid,'text-yellow-500':v$.code.$error}" :name="`heroicons-solid:${v$.code.$error?'exclamation':'check-circle'}`"
            />
          </div>
        </div>
        <button
            type="submit"
            class="rounded border-0 bg-blue-500 py-2 px-8 font-bold text-[#213547] transition-colors duration-500 hover:bg-blue-600 focus:outline-none"
        >
          Submit
        </button>
      </form>
    </section>
  </main>
</template>

<script setup>
import {required, email, maxLength, minLength, sameAs, helpers} from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";

const formData = reactive({
  email:'',
  password:'',
  confirmPassword:'',
  code:'',
})
const form =ref()
const rules = computed(()=>{
  return{
    email: {
      required: helpers.withMessage('The email field is required', required),
      email: helpers.withMessage('Invalid email format!', email)
    },
    password:{
      required: helpers.withMessage('The password field is required', required),
      minLength: minLength(6)
    },
    confirmPassword:{
      required: helpers.withMessage('The password comfirmation field is required', required),
      sameAs: helpers.withMessage('Password don\'t match',sameAs(formData.password))
    },
    code:{
      required: helpers.withMessage('The code field is required', required),
      maxLength: maxLength(6),
      minLength:minLength(6)
    }
  }
})

const v$ = useVuelidate(rules, formData)

const submitForm = ()=>{
  console.log(formData.value)
  v$.value.$validate();
  if(!v$.value.$error){
    formData.email = ''
    formData.password = ''
    formData.confirmPassword = ''
    formData.code = ''
    v$.value.$reset()
  }
}

</script>