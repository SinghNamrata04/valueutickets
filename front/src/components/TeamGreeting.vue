<template>
  <div class="greeting-container">
    <h1>Hello, Team!</h1>
    <p>You’re doing amazing work. Keep pushing forward—we’ve got this! 💪✨</p>
  </div>
</template>

<script>
export default {
  name: "TeamGreeting",
};
</script>

<style scoped>
.greeting-container {
  text-align: center;
  margin: 50px auto;
  padding: 20px;
  max-width: 400px;
  border: 2px solid #4caf50;
  border-radius: 12px;
  background-color: #e8f5e9;
  color: #388e3c;
  font-family: Arial, sans-serif;
}

h1 {
  font-size: 2rem;
  margin-bottom: 10px;
}

p {
  font-size: 1.2rem;
}
</style>
