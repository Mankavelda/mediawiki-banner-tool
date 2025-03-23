<template>
  <div class="container">
    <!-- Banner Section -->
    <div class="banner" :style="bannerStyle">
      <h2 :style="{ color: textColor }">{{ text }}</h2>
      
      <!-- Image Section -->
      <img v-if="imageSrc && imageExists" :src="imageSrc" alt="Banner Image" class="banner-image" />
      <img v-else :src="defaultImage" alt="Default Banner Image" class="banner-image" />
      
      <!-- Error Message if Image is Invalid -->
      <p v-if="!imageExists && imageSrc">Image not found. Please check the URL.</p>
    </div>

    <!-- Controls Section -->
    <form class="controls">
      <label for="bgColor">Change Background Color:</label>
      <input id="bgColor" type="color" v-model="bgColor" class="input-color" />

      <label for="text">Change Text:</label>
      <input id="text" type="text" v-model="text" placeholder="Update banner text" class="input-text" />

      <label for="image">Change Image:</label>
      <input id="image" type="text" v-model="imageSrc" placeholder="Enter image URL" class="input-text" />
    </form>
  </div>
</template>

<script>
import defaultImage from '@/assets/banner_image.png';

export default {
  data() {
    return {
      bgColor: "#f0f0f0",
      text: "I love open-source!",
      imageSrc: "",
      defaultImage,
    };
  },
  computed: {
    bannerStyle() {
      return {
        backgroundColor: this.bgColor,
      };
    },
    textColor() {
      // Ensuring contrast by switching between black and white text
      return this.getContrastColor(this.bgColor);
    },
    imageExists() {
      if (!this.imageSrc) return true;
      const image = new Image();
      image.src = this.imageSrc;
      return image.complete && image.naturalHeight !== 0;
    },
  },
  methods: {
    getContrastColor(hex) {
      // Convert hex to RGB
      const r = parseInt(hex.slice(1, 3), 16);
      const g = parseInt(hex.slice(3, 5), 16);
      const b = parseInt(hex.slice(5, 7), 16);
      // Calculate luminance
      const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
      return luminance > 0.5 ? '#000000' : '#FFFFFF';
    },
  },
};
</script>

<style scoped>
/* Main Container */
.container {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}

/* Banner Styling */
.banner {
  width: 100%;
  padding: 40px;
  text-align: center;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, color 0.3s ease;
}

.banner h2 {
  font-size: 2rem;
  transition: color 0.3s ease;
}

.banner-image {
  max-width: 100%;
  height: auto;
  margin-top: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* Controls Styling */
.controls {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 20px;
}

.controls label {
  font-size: 1rem;
  font-weight: bold;
}

.input-color {
  width: 60px;
  padding: 5px;
  border: 2px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease;
}

.input-text {
  padding: 10px;
  font-size: 1rem;
  border-radius: 5px;
  border: 2px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  width: 100%;
}

.input-text:focus, .input-color:focus {
  border-color: #007BFF;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
  outline: none;
}

@media (max-width: 600px) {
  .container {
    padding: 15px;
  }

  .banner h2 {
    font-size: 1.5rem;
  }

  .input-text, .input-color {
    width: 100%;
  }
}
</style>
