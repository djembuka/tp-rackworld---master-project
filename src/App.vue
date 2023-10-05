<template>
  <form action="">
    <div v-for="(num, index) in Array(formControlsNum)" :key="index">
      <form-control
        v-if="formControls[index]"
        :index="index"
        :name="formControls[index].name"
        :options="formControls[index].options"
        @formcontrolchange="formControlChange"
      ></form-control>

      <form-control-disabled v-else></form-control-disabled>
    </div>

    <button type="submit" :disabled="formControls.length !== formControlsNum">
      Подобрать
    </button>
    <button @click="clearForm($event)">Очистить</button>
  </form>
</template>

<script>
//import controlsData from '@/controlsData.json';
import FormControl from './components/FormControl.vue';
import FormControlDisabled from './components/FormControlDisabled.vue';

export default {
  name: 'App',
  components: {
    FormControl,
    FormControlDisabled,
  },
  data() {
    return {
      options: [{ country: 'Canada', code: 'CA' }],
      formControlsNum: 5,
      formControls: [
        {
          name: 'param1',
          type: 'select',
          options: [
            { value: 'v1', text: 'Value 1' },
            { value: 'v2', text: 'Value 2' },
            { value: 'v3', text: 'Value 3' },
          ],
        },
      ],
    };
  },
  methods: {
    formControlChange(index) {
      (async () => {
        try {
          let timestamp = new Date().getTime();
          let response = await fetch(
            `../src/controlsData.json?id=${timestamp}`
          );
          let controlObject = await response.json();

          this.formControls.splice(
            index + 1,
            this.formControls.length,
            controlObject
          );
        } catch (err) {
          console.log(err);
        }
      })();
    },
    clearForm(e) {
      e.preventDefault();
      (async () => {
        let timestamp = new Date().getTime();
        let response = await fetch(
          `@/controlObjectClearForm.json?${timestamp}`
        );
        let controlObject = await response.json();
        console.log(controlObject);
        this.formControls = [controlObject];
        console.log(this.formControls);
      })();
    },
  },
};
</script>
