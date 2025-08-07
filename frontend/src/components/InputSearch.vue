<style scoped>
.input-group {
  max-width: 400px; 
  margin-left: auto; 
}

.form-control {
  border: 2px solid #2196F3 ;
  border-right: none;
  border-radius: 10px 0 0 10px; 
  padding: 10px;
  font-size: 16px;
}

.form-control:focus {
  box-shadow: none;
  border-color: #064ca2 ;
}

.btn-outline-secondary {
  border: 2px solid #2196F3;
  border-left: none;
  border-radius: 0 10px 10px 0; 
  background-color: #2196F3;
  color: white;
  font-weight: bold;
}

.btn-outline-secondary:hover {
  background-color: #064ca2;
  border-color: #064ca2;
}
</style>

<template>
  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Tìm kiếm..." :value="modelValue"
      @input="updateModelValue" @keyup.enter="submit" />
    <button class="btn btn-outline-secondary" @click="submit">Tìm</button>
  </div>
</template>

<script setup>
defineProps({
  modelValue: { type: String, default: "" },
});
const emit = defineEmits(["update:modelValue", "submit"]);

const updateModelValue = (e) => {
  emit("update:modelValue", e.target.value);
};

const submit = () => {
  emit("submit");
};
</script>