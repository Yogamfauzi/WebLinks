<template>
  <div class="luxury-bg min-h-screen relative overflow-hidden">
    <!-- Animated Background Elements -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div 
        v-for="(particle, index) in particles" 
        :key="index"
        class="absolute w-2 h-2 bg-gold rounded-full opacity-30"
        :style="{ 
          left: particle.x + '%', 
          top: particle.y + '%',
          animationDelay: particle.delay + 's',
          animationDuration: particle.duration + 's'
        }"
        :class="particle.animation"
      ></div>
    </div>

    <!-- Luxury Pattern Overlay -->
    <div class="absolute inset-0 luxury-pattern opacity-10"></div>

    <div class="relative z-10 flex flex-col items-center justify-center min-h-screen px-6 py-12">
      <!-- Profile Section -->
      <div class="text-center mb-12">
        <!-- Profile Image with Luxury Border -->
        <div class="profile-container mb-12" @mouseenter="startProfileAnimation" @mouseleave="stopProfileAnimation">
          <div class="profile-ring" :class="{ 'animate-spin-slow': isProfileAnimated }">
            <div class="profile-inner">
              <img 
                :src="profileImage" 
                alt="Profile Picture" 
                class="w-full h-full object-cover rounded-full transition-transform duration-500 hover:scale-110"
                @error="handleImageError"
              >
            </div>
          </div>
          <!-- Glow Effect -->
          <div class="profile-glow"></div>
        </div>

        <!-- Name and Bio -->
        <h1 class="luxury-title text-5xl md:text-6xl font-bold mt-4 mb-4 animate-fade-in">
          Probolinggo Crypto Community
        </h1>
        <p class="text-gold-light text-lg md:text-xl mb-2 opacity-90 animate-fade-in-delay">
          Hallo Trader Muda
        </p>
        <p class="text-gray-400 text-sm md:text-base animate-fade-in-delay-2">
          Silahkan join untuk mendapatkan informasi lebih lanjut dari kami
        </p>
      </div>

      <!-- Links Section -->
      <div class="w-full max-w-md space-y-6">
        <div 
          v-for="(link, index) in links" 
          :key="index"
          class="link-card group cursor-pointer transform transition-all duration-500 hover:scale-105"
          :style="{ animationDelay: (index * 0.1) + 's' }"
          @click="openLink(link.url)"
          @mouseenter="playHoverSound"
        >
          <div class="link-inner p-6 rounded-2xl relative overflow-hidden">
            <!-- Shine Effect -->
            <div class="shine-effect"></div>
            
            <!-- Content -->
            <div class="relative z-10 flex items-center space-x-4">
              <!-- Icon -->
              <div class="link-icon-wrapper">
                <div 
                  class="w-14 h-14 rounded-xl flex items-center justify-center transition-all duration-300 group-hover:scale-110 group-hover:rotate-6"
                  :style="{ background: link.iconBg }"
                >
                  <i :class="link.icon" class="text-2xl text-white"></i>
                </div>
              </div>

              <!-- Text Content -->
              <div class="flex-1 text-left">
                <h3 class="text-white font-semibold text-lg mb-1 group-hover:text-gold transition-colors">
                  {{ link.title }}
                </h3>
                <p class="text-gray-400 text-sm group-hover:text-gray-300 transition-colors">
                  {{ link.description }}
                </p>
              </div>

              <!-- Arrow -->
              <div class="link-arrow">
                <svg class="w-6 h-6 text-gold transform group-hover:translate-x-2 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
                </svg>
              </div>
            </div>

            <!-- Hover Gradient -->
            <div class="hover-gradient"></div>
          </div>
        </div>
      </div>

      <!-- Footer Info -->
      <div class="text-center mt-12 space-y-2 animate-fade-in-delay-3">
        <div class="text-gold font-medium">
          ✨ {{ footerText }} ✨
        </div>
        <div class="text-gray-500 text-sm">
          © {{ currentYear }} @yogamfauziii. All rights reserved.
        </div>
      </div>
    </div>

    <!-- Click Ripple Effect -->
    <div 
      v-if="ripple.show" 
      class="ripple-effect"
      :style="{ left: ripple.x + 'px', top: ripple.y + 'px' }"
    ></div>
  </div>
</template>

<script>
import profileImage from '../images/logopcc.jpg'; // pastikan path sesuai struktur folder Anda

export default {
  name: 'LuxuryLinktree',
  data() {
    return {
      footerText: 'Thank you for visiting',
      profileImage,
      isProfileAnimated: false,
      currentYear: new Date().getFullYear(),

      links: [
        {
          title: 'Grub WhatsApp',
          description: 'Join our exclusive community',
          url: 'https://chat.whatsapp.com/GqoopaasyEIH5gBoNplodt',
          icon: 'fab fa-whatsapp',
          iconBg: 'linear-gradient(135deg, #25D366, #128C7E)'
        },
        {
          title: 'Saluran WhatsApp',
          description: 'Join our exclusive community',
          url: 'https://whatsapp.com/channel/0029VbAyjeg1NCrVMJo06S3z',
          icon: 'fab fa-whatsapp',
          iconBg: 'linear-gradient(135deg, #25D366, #128C7E)'
        },
        {
          title: 'Instagram',
          description: 'Follow',
          url: 'https://www.instagram.com/prob.cryptocommunity?igsh=aTdrdjMyaGZkd2U2',
          icon: 'fab fa-instagram',
          iconBg: 'linear-gradient(135deg, #E4405F, #833AB4, #F77737)'
        }
      ],

      particles: [],
      ripple: {
        show: false,
        x: 0,
        y: 0
      }
    };
  },

  mounted() {
    this.generateParticles();
    this.loadFontAwesome();
    document.addEventListener('click', this.createRipple);
  },

  beforeUnmount() {
    document.removeEventListener('click', this.createRipple);
  },

  methods: {
    generateParticles() {
      for (let i = 0; i < 20; i++) {
        this.particles.push({
          x: Math.random() * 100,
          y: Math.random() * 100,
          delay: Math.random() * 5,
          duration: 3 + Math.random() * 4,
          animation: Math.random() > 0.5 ? 'animate-pulse' : 'animate-ping'
        });
      }
    },
    loadFontAwesome() {
      if (!document.querySelector('link[href*="font-awesome"]')) {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css';
        document.head.appendChild(link);
      }
    },
    startProfileAnimation() {
      this.isProfileAnimated = true;
    },
    stopProfileAnimation() {
      this.isProfileAnimated = false;
    },
    openLink(url) {
      window.open(url, '_blank');
    },
    playHoverSound() {
      // Optional sound
    },
    handleImageError() {
      this.profileImage = 'https://via.placeholder.com/400x400/FFD700/000000?text=Profile';
    },
    createRipple(event) {
      this.ripple = {
        show: true,
        x: event.clientX - 25,
        y: event.clientY - 25
      };
      setTimeout(() => {
        this.ripple.show = false;
      }, 600);
    }
  }
};
</script>

<style scoped>
.luxury-bg {
  background: 
    radial-gradient(circle at 20% 80%, rgba(255, 215, 0, 0.15) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(255, 215, 0, 0.1) 0%, transparent 50%),
    linear-gradient(135deg, #000000 0%, #1a1a1a 50%, #000000 100%);
}

.luxury-pattern {
  background-image: 
    radial-gradient(circle at 2px 2px, rgba(255, 215, 0, 0.3) 1px, transparent 0);
  background-size: 50px 50px;
}

.profile-container {
  position: relative;
  width: 200px;
  height: 200px;
  margin: 0 auto;
}

.profile-ring {
  position: absolute;
  inset: -6px;
  background: conic-gradient(from 0deg, #FFD700, #FFA500, #FFD700, #B8860B, #FFD700);
  border-radius: 50%;
  padding: 6px;
  transition: all 0.3s ease;
}

.profile-inner {
  width: 100%;
  height: 100%;
  background: #000;
  border-radius: 50%;
  overflow: hidden;
  position: relative;
  z-index: 2;
}

.profile-glow {
  position: absolute;
  inset: -20px;
  background: radial-gradient(circle, rgba(255, 215, 0, 0.4) 0%, transparent 70%);
  border-radius: 50%;
  z-index: 1;
  animation: pulse-glow 3s ease-in-out infinite alternate;
}

.luxury-title {
  background: linear-gradient(135deg, #FFD700, #FFA500, #FFD700);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-shadow: 0 0 30px rgba(255, 215, 0, 0.5);
  line-height: 1.2;
  padding-bottom: 0.2em;
}

.link-card {
  animation: slideInUp 0.8s ease-out forwards;
  opacity: 0;
  transform: translateY(30px);
}

.link-inner {
  background: linear-gradient(135deg, 
    rgba(255, 215, 0, 0.1) 0%, 
    rgba(26, 26, 26, 0.9) 30%, 
    rgba(0, 0, 0, 0.95) 100%);
  border: 2px solid transparent;
  background-clip: padding-box;
  backdrop-filter: blur(20px);
  position: relative;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.link-card:hover .link-inner {
  border-color: rgba(255, 215, 0, 0.5);
  box-shadow: 
    0 20px 40px rgba(255, 215, 0, 0.2),
    inset 0 1px 0 rgba(255, 215, 0, 0.1);
  transform: translateY(-5px);
}

.shine-effect {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, 
    transparent, 
    rgba(255, 215, 0, 0.2), 
    transparent);
  transition: left 0.6s;
}

.link-card:hover .shine-effect {
  left: 100%;
}

.hover-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, 
    rgba(255, 215, 0, 0.05), 
    transparent);
  opacity: 0;
  transition: opacity 0.3s;
  border-radius: inherit;
}

.link-card:hover .hover-gradient {
  opacity: 1;
}

.text-gold {
  color: #FFD700;
}

.text-gold-light {
  color: #FFF8DC;
}

.animate-spin-slow {
  animation: spin 8s linear infinite;
}

.animate-fade-in {
  animation: fadeIn 1s ease-out;
}

.animate-fade-in-delay {
  animation: fadeIn 1s ease-out 0.2s both;
}

.animate-fade-in-delay-2 {
  animation: fadeIn 1s ease-out 0.4s both;
}

.animate-fade-in-delay-3 {
  animation: fadeIn 1s ease-out 0.6s both;
}

.ripple-effect {
  position: fixed;
  width: 50px;
  height: 50px;
  background: radial-gradient(circle, rgba(255, 215, 0, 0.6) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  animation: ripple 0.6s ease-out;
}

@keyframes pulse-glow {
  0% { opacity: 0.5; transform: scale(1); }
  100% { opacity: 0.8; transform: scale(1.1); }
}

@keyframes slideInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes ripple {
  0% {
    transform: scale(0);
    opacity: 1;
  }
  100% {
    transform: scale(4);
    opacity: 0;
  }
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>