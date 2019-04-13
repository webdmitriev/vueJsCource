<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <!--  -->
      <section>
        <div class="container">
          <!-- First modal -->
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
          <modals v-show="modalFirst" title="First modal" @close="modalFirst = false">
            <!-- body -->
            <div slot="body">
              <p>Text Text Text Text Text Text Text Text Text</p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well Done!</button>
            </div>
            <!-- /body -->
          </modals>

          <!-- Second modal -->
          <button
            class="btn btnPrimary"
            @click="modalSecond.show = !modalFirst"
          >Show modal with form</button>
          <modals
            title="Modal with form"
            v-show="modalSecond.show"
            @close="modalSecond.show = false"
          >
            <!-- body -->
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" v-model="modalSecond.name">
                <label>Email:</label>
                <input type="email" v-model="modalSecond.email">
                <button class="btn btnPrimary">Submit</button>
              </form>
            </div>
            <!-- /body -->
          </modals>

          <!-- modal with validate -->
          <button
            class="btn btnPrimary"
            @click="modalValidate = !modalValidate"
          >Show modal with form + Validate</button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false"/>
        </div>
      </section>
      <!--  -->
    </div>
  </div>
</template>

<script>
import modals from "@/components/UI/Modal.vue";
import modalValidate from "@/components/ModalValidate.vue";

export default {
  components: {
    modals,
    modalValidate
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: "",
        email: ""
      },
      modalValidate: false
    };
  },
  methods: {
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email
      });
      this.modalSecond.name = "";
      this.modalSecond.email = "";
      this.modalSecond.show = false;
    }
  }
};
</script>