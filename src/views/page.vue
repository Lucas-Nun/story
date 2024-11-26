<template>
  <div class="top_Bar">
    <h1>Murder Mystery</h1>
  </div>

  <br><br><br><br>


  <section class="story">
    <!-- Display the story text dynamically -->
    <p>{{ currentStory.text }}</p>

    <!-- Display the image dynamically if available -->
    <img v-if="currentStory.image" :src="currentStory.image" alt="Story image" class="story-image" />
    <!-- Display the current prompt dynamically -->
    <section class="action">
      <p>{{ currentStory.prompt }}</p>
    </section>
  </section>

  <section 
  class="choices" 
  :class="{ 'no-choices': !hasChoices }">
    <div class="choices-container">
      <!-- Dynamically display choices based on the story data -->
      <div v-if="hasChoices">
        <div v-if="currentStory.choice1_text">
          <button class="buttonexample" @click="makeChoice(currentStory.choice1_next)">{{ currentStory.choice1_text }}</button>
        </div>
        <div v-if="currentStory.choice2_text">
          <button class="buttonexample"  @click="makeChoice(currentStory.choice2_next)">{{ currentStory.choice2_text }}</button>
        </div>
        <div v-if="currentStory.choice3_text">
          <button class="buttonexample"  @click="makeChoice(currentStory.choice3_next)">{{ currentStory.choice3_text }}</button>
        </div>
      </div>
      <div v-else>
        
      </div>
    </div>
  </section>

  <section class="saveslots">
    <h1>Story Progress</h1>
    <div v-for="(slot, index) in saveSlots" :key="index" class="save-slot">
      <p>Slot {{ index + 1 }} - {{ slot ? 'Saved' : 'Empty' }}</p>
      <button @click="saveProgress(index)">Save</button>
      <button @click="loadProgress(index)" :disabled="!slot">Load</button>
      <button @click="resetSlot(index)" :disabled="!slot">Reset</button>
    </div>
  </section>

  <br> <br> <br>
</template>

<script>
import Papa from "papaparse";

export default {
  data() {
    return {
      currentIndex: 0, // Tracks the current story index
      story: [], // Stores the story data loaded from CSV
      saveSlots: Array(3).fill(null), // Initializes save slots
    };
  },
  computed: {
    currentStory() {
      console.log("Current Story:", this.story[this.currentIndex]); // Debugging
      return this.story[this.currentIndex] || {};
    },
    hasChoices() {
      return (
        this.currentStory.choice1_text ||
        this.currentStory.choice2_text ||
        this.currentStory.choice3_text
      );
    },
  },
  methods: {
    loadStory() {
      fetch("/narrative.csv")
        .then((response) => {
          if (!response.ok) {
            throw new Error(`Failed to fetch CSV: ${response.statusText}`);
          }
          return response.text();
        })
        .then((csvText) => {
          Papa.parse(csvText, {
            header: true,
            skipEmptyLines: true,
            complete: (result) => {
              console.log("Parsed CSV:", result.data); // Debugging
              this.story = result.data;
            },
            error: (err) => {
              console.error("Error parsing CSV:", err);
            },
          });
        })
        .catch((error) => console.error("Error loading CSV:", error));
    },
    makeChoice(nextIndex) {
      if (nextIndex !== undefined && !isNaN(nextIndex)) {
        this.currentIndex = parseInt(nextIndex);
      } else {
        console.error("Invalid choice index:", nextIndex);
      }
    },
    saveProgress(slotIndex) {
      this.saveSlots[slotIndex] = { currentIndex: this.currentIndex };
      localStorage.setItem("saveSlots", JSON.stringify(this.saveSlots));
    },
    loadProgress(slotIndex) {
      const savedSlot = this.saveSlots[slotIndex];
      if (savedSlot) {
        this.currentIndex = savedSlot.currentIndex;
      }
    },
    resetSlot(slotIndex) {
      this.saveSlots[slotIndex] = null;
      localStorage.setItem("saveSlots", JSON.stringify(this.saveSlots));
    },
    loadSaveSlots() {
      const savedSlots = localStorage.getItem("saveSlots");
      if (savedSlots) {
        this.saveSlots = JSON.parse(savedSlots);
      }
    },
  },
  mounted() {
    this.loadSaveSlots(); // Load save slots when the component is mounted
    this.loadStory(); // Load the story from CSV when the component is mounted
  },
};
</script>

