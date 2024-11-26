<template>
  <div class="settings-container">
    <div class="top_Bar">
      <h1>Settings</h1>
    </div>

    <br><br><br><br><br>

    <div class="settings">
      <h2>Background Settings</h2>
      <div class="settings-item">
        <a>Toggle Background:</a>
        <button @click="toggleBackground(false)">Off</button>
        <button @click="toggleBackground(true)">On</button>
      </div>
    </div>

    <div class="settings">
      <h2>Appearance Settings</h2>
      <div class="settings-item">
        <a>Dark Mode:</a>
        <button @click="toggleDarkMode(false)">Off</button>
        <button @click="toggleDarkMode(true)">On</button>
      </div>
    </div>

    <div class="settings">
      <h2>Text Settings</h2>
      <div class="settings-item">
        <a>Adjust Font Size:</a>
        <button @click="adjustFontSize(false)">Decrease</button>
        <button @click="adjustFontSize(true)">Increase</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDarkMode: true, // Tracks dark mode status
      fontSize: 16, // Default font size
    };
  },
  methods: {
    toggleBackground(isOn) {
      const body = document.body;
      if (isOn) {
        body.style.backgroundImage = "url('wallpaper.jpg')";
      } else {
        body.style.backgroundImage = "none";
      }
    },
    toggleDarkMode(isOn) {
      const body = document.body;
      this.isDarkMode = isOn;
      if (this.isDarkMode) {
        body.style.backgroundColor = "#141414"; // Dark background
        body.style.color = "#ffffff"; // Light text
      } else {
        body.style.backgroundColor = "#ffffff"; // Light background
        body.style.color = "#000000"; // Dark text
      }
    },

  },
};
</script>

