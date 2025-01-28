<template>
  <div>
    <nav>
      <div class="logo">
        <img :src="logo" alt="Logo" />
      </div>
      <div class="menu-icon" @click="toggleMenu">
        <div></div>
        <div></div>
        <div></div>
      </div>
      <ul :class="['nav-links', { active: isMenuActive }]">
        <li><a href="#">Home</a></li>
        <li><a href="#">My Booking</a></li>
        <li><a href="#">Blogs</a></li>
        <li><a href="#">Packages</a></li>
        <li><a href="#">Services</a></li>
        <li><button class="login-btn">Login or Signup</button></li>
        <li>
          <div class="vertical-bar"></div>
        </li>
        <li class="dropdown" @click.stop="toggleDropdown">
          <div class="dropdown-toggle">
            <img :src="selectedFlag" alt="Flag" />
            <span
          class="dropdown-icon ml-2"
          :class="{ rotate: isRotated }"
        >&or;</span>
          </div>
          <ul :class="['dropdown-menu', { show: isDropdownActive }]">
            <li
              v-for="(flag, index) in flags"
              :key="index"
              @click="changeFlag(flag.url)"
            >
              <img :src="flag.url" :alt="flag.name" />
            </li>
          </ul>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import logoPath from "@/assets/logo-nobg.png";

export default {
  data() {
    return {
      logo: logoPath, 
      isMenuActive: false,
      isDropdownActive: false,
      isRotated: false, 
      selectedFlag: "https://upload.wikimedia.org/wikipedia/commons/f/fc/Flag_of_Mexico.svg", 
      flags: [
        { name: "France", url: "https://upload.wikimedia.org/wikipedia/en/c/c3/Flag_of_France.svg" },
        { name: "USA", url: "https://upload.wikimedia.org/wikipedia/en/a/a4/Flag_of_the_United_States.svg" },
        { name: "India", url: "https://upload.wikimedia.org/wikipedia/en/4/41/Flag_of_India.svg" },
        { name: "Mexico", url: "https://upload.wikimedia.org/wikipedia/commons/f/fc/Flag_of_Mexico.svg" },
      ],
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuActive = !this.isMenuActive;
    },
    toggleDropdown() {
      this.isDropdownActive = !this.isDropdownActive;
      this.isRotated = !this.isRotated;
    },
    changeFlag(flagUrl) {
      this.selectedFlag = flagUrl;
      this.isDropdownActive = true; // Close the dropdown after selecting a flag
    },
    closeDropdownOnClickOutside(event) {
      if (
        !event.target.closest(".dropdown") &&
        !event.target.closest(".menu-icon")
      ) {
        this.isDropdownActive = false;
      }
    },
  },
  mounted() {
    window.addEventListener("click", this.closeDropdownOnClickOutside);
  },
  beforeUnmount() {
    window.removeEventListener("click", this.closeDropdownOnClickOutside);
  },
};
</script>

<style scoped>
@import url(https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css);

nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 50px;
  position: relative;
  /* background-color: #0056b3; */
}

.logo img {
  width: 100px;
  /* height: auto; */
  /* padding-left: 50px; */
}

.nav-links {
  display: flex;
  align-items: center;
  list-style: none;
}

.nav-links li {
  margin: 0 20px;
}

.nav-links a {
  text-decoration: none;
  color: black;
  font-size: 20px;
  font-weight: bold;
}

.nav-links a:hover {
  text-decoration: underline;
}

.login-btn {
  padding: 8px 18px;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 18px;
  background-color: #007bff;
}

.login-btn:hover {
  background-color: #0056b3;
}

.vertical-bar {
  width: 1px;
  height: 70px;
  background-color: black;
}

.dropdown {
  position: relative;
}

.dropdown img {
  width: 35px;
  height: 35px;
  margin-right: -0px;
}

.dropdown-toggle {
  display: flex;
  align-items: center;
  /* font-size: 30px; */
  cursor: pointer;
  
}
.dropdown-toggle::after {
      display: none;
    }

.dropdown-icon {
  font-size: 30px;
  /* margin-left: -15px; */
  font-weight:  bolder;
  transition: transform 0.3s ease; 
  margin-top: -8px;

}
.dropdown-icon.rotate {
      transform: rotate(180deg);
    }




.dropdown-menu {
  display: none;
  position: absolute;
  /* top: 150px; */
  left: -30px; /* Set left to 50px */
  background-color: rgb(198, 6, 6);
  /* border-radius: 5px; */
  /* z-index: 10; */
  background-color: transparent; 
  border: none; 
  box-shadow: none; 
}


.dropdown-menu.show {
  display: block;
}

.dropdown-menu li {
  padding: 5px 10px;
  cursor: pointer;
  background-color: transparent; 
  /* background-color: #0056b3 */
}



.menu-icon {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 25px;
  height: 18px;
  cursor: pointer;
}

.menu-icon div {
  width: 100%;
  height: 3px;
  background-color:black;
}

/* Tablet and Mobile adjustments */
@media (max-width: 1024px) {  /* For tablets */
  .menu-icon {
    display: flex;
  }

  .nav-links {
    display: none;
    flex-direction: column;
    align-items: flex-start;
    width: 100%;
    padding: 15px;
    position: absolute;
    top: 90px;
    left: 0;
   
  }

  .nav-links.active {
    display: flex;
  }

  .nav-links li {
    margin: 10px 0;
  }

  .dropdown-menu 
  {
    padding-left: 20px;
    /* background-color: yellow; */
  }

  .vertical-bar {
    display: none;
  }
}


@media (max-width: 768px) {  /* For smaller tablets and larger phones */
  
  
  .logo img {
    width: 100px;
    margin-top: 20px;
  }

  .dropdown img {
    width: 35px;
   
  }

  .nav-links a {
    font-size: 18px;
  }

  .login-btn {
    font-size: 16px;
    /* display: none; */
  }

  .dropdown-icon {
    font-size: 20px;
  }

  .dropdown-menu {
    width: 100px;
    
   
    
  }
}

@media (max-width: 480px) {  /* For phones */
  .logo img {
    width: 60px;
   padding: -50px;
  }

  .dropdown img {
    width: 30px;
  }

  .nav-links a {
    font-size: 16px;
  }

  .login-btn {
    font-size: 14px;
  }

  .dropdown-icon {
    font-size: 18px;
  }
}
</style>
