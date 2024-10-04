<template>
    <div>
    <v-btn @click="handleClick">
        Click me
    </v-btn>
    <v-combobox
    v-model="city2"
    label="QUẢN TRỊ VIÊN"
    :items="cities"
    ></v-combobox>
  <i class="fa-solid fa-magnifying-glass"></i>
  <v-pagination
      v-model="page"
      :length="4"
      rounded="circle"
    ></v-pagination>

    <v-pagination
      v-model="page"
      :length="4"
      rounded="0"
    ></v-pagination>
    <v-alert title="XÁC NHẬN" text="BẠN CÓ XÁC NHẬN XÓA KO ?"></v-alert>
    <v-card title="Card title" subtitle="Subtitle" text="...">
  <v-card-actions>
    <v-btn>Click me</v-btn>
  </v-card-actions>
</v-card>
<v-dialog max-width="500">
  <template v-slot:activator="{ props: activatorProps }">
    <v-btn
      v-bind="activatorProps"
      color="surface-variant"
      text="XÁC NHẬN"
      variant="flat"
    ></v-btn>
  </template>

  <template v-slot:default="{ isActive }">
    <v-card title="XÁC NHÂN">
      <v-card-text>
      BẠN CÓ MUỐN LÀM MỚI DỮ LIỆU KO ?
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn
          text="XÁC NHẬN"
          @click="isActive.value = false"
        ></v-btn>
      </v-card-actions>
    </v-card>
  </template>
</v-dialog>

<v-card>
    <v-tabs
      v-model="tab"
      bg-color="primary"
    >
      <v-tab value="one">Item One</v-tab>
      <v-tab value="two">Item Two</v-tab>
      <v-tab value="three">Item Three</v-tab>
    </v-tabs>

    <v-card-text>
      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="one">
          One
        </v-tabs-window-item>

        <v-tabs-window-item value="two">
          Two
        </v-tabs-window-item>

        <v-tabs-window-item value="three">
          Three
        </v-tabs-window-item>
      </v-tabs-window>
    </v-card-text>
  </v-card>

  <v-row justify="center">
    <v-col
      cols="12"
      lg="6"
      md="8"
      sm="10"
    >
      <v-card ref="form">
        <v-card-text>
          <v-text-field
            ref="name"
            v-model="name"
            :error-messages="errorMessages"
            :rules="[() => !!name || 'This field is required']"
            label="Full Name"
            placeholder="John Doe"
            required
          ></v-text-field>
          <v-text-field
            ref="address"
            v-model="address"
            :rules="[
              () => !!address || 'This field is required',
              () => !!address && address.length <= 25 || 'Address must be less than 25 characters',
              addressCheck
            ]"
            counter="25"
            label="Address Line"
            placeholder="Snowy Rock Pl"
            required
          ></v-text-field>
          <v-text-field
            ref="city"
            v-model="city"
            :rules="[() => !!city || 'This field is required', addressCheck]"
            label="City"
            placeholder="El Paso"
            required
          ></v-text-field>
          <v-text-field
            ref="state"
            v-model="state"
            :rules="[() => !!state || 'This field is required']"
            label="State/Province/Region"
            placeholder="TX"
            required
          ></v-text-field>
          <v-text-field
            ref="zip"
            v-model="zip"
            :rules="[() => !!zip || 'This field is required']"
            label="ZIP / Postal Code"
            placeholder="79938"
            required
          ></v-text-field>
          <v-autocomplete
            ref="country"
            v-model="country"
            :items="countries"
            :rules="[() => !!country || 'This field is required']"
            label="Country"
            placeholder="Select..."
            required
          ></v-autocomplete>
        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          <v-btn variant="text">
            Cancel
          </v-btn>
          <v-spacer></v-spacer>
          <v-slide-x-reverse-transition>
            <v-tooltip
              v-if="formHasErrors"
              location="left"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  class="my-0"
                  icon
                  v-bind="attrs"
                  v-on="on"
                  @click="resetForm"
                >
                  <v-icon>mdi-refresh</v-icon>
                </v-btn>
              </template>
              <span>Refresh form</span>
            </v-tooltip>
          </v-slide-x-reverse-transition>
          <v-btn
            color="primary"
            variant="text"
            @click="submit"
          >
            Submit
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
    </div>

</template>

<script setup>
import { reactive , ref , computed, watch} from 'vue';
const tab = ref(null)
const city2 = ref("");
const cities = reactive([
'THÔNG TIN CÁ NHÂN','ĐỔI MẬT KHẨU','ĐĂNG XUẤT'
]);
const handleClick = ()=>{
    console.log(
        "Clicked"
    );
};


const countries = ['Afghanistan', 'Virgin Islands (US)', 'Yemen', 'Zambia', 'Zimbabwe']

const errorMessages = ref('')
const name = ref(null)
const address = ref(null)
const city1 = ref(null)
const state = ref(null)
const zip = ref(null)
const country = ref(null)
const formHasErrors = ref(false)

const form = computed(() => ({
  name: name.value,
  address: address.value,
  city: city1.value,
  state: state.value,
  zip: zip.value,
  country: country.value,
}))

watch(name, () => {
  errorMessages.value = ''
})

function addressCheck() {
  errorMessages.value = address.value && !name.value
    ? `Hey! I'm required`
    : ''
  return true
}

function resetForm() {
  errorMessages.value = []
  formHasErrors.value = false

  Object.keys(form.value).forEach(f => {
    if (form.value[f] !== null && form.value[f].reset) {
      form.value[f].reset()
    }
  })
}

function submit() {
  formHasErrors.value = false

  Object.keys(form.value).forEach(f => {
    if (!form.value[f]) {
      formHasErrors.value = true
    }
    if (form.value[f] !== null && form.value[f].validate) {
      form.value[f].validate(true)
    }
  })
}

</script>

<style>

</style>