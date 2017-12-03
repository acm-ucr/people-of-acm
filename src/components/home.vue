<template>
  <b-container class="h-100 mt-5">
    <div class="text-center header row">
      <div class="col-3">
        <button @click="scroll(isScrolling); isScrolling = !isScrolling" class="scroll-toggle btn btn-sm btn-outline-primary">Turn On Scroll</button>
      </div>
      <div class="col-xs-auto">
        <h1>PEOPLE OF ACM</h1>
      </div>
      <div class="col signinlink">
        <a href="http://acmucr.org/checkin">
          <span class="checkin-desktop">acmucr.org/checkin</span>
          <span class="checkin-mobile">Check In</span>
          </a>
      </div>
    </div>
    <b-card-group columns>
      <b-card v-for='p in people'
        :key='p.name'
        :title='p.name'
        :img-src='`static/people/${p.image}`'
        img-fluidz
        img-alt="image"
        img-top
        class="jcard">
          <p class="card-text">
            {{ p.bio }}
          </p>
          <a :href="p.website" class="bio">
            <p class="text-center">
              <span class="btn btn-primary">Website</span>
            </p>
          </a>
        </b-card>
    </b-card-group>
  </b-container>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Roboto:900");
.btn-outline-primary:hover, .btn-outline-primary:not([disabled]):not(.disabled):active, .btn-outline-primary:not([disabled]):not(.disabled).active, .show > .btn-outline-primary.dropdown-toggle {
  background-color: white;
  border-color: black;
  color: black;
}
.scroll-toggle {
  position: relative;
  margin-top: 8px;
  color: black;
  border: 1px black solid;
}
.signinlink {
  position: relative;
  margin-top: 13px;
}
.signinlink a {
  color: black;
}

.checkin-mobile {
  display: none;
}

.header {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 2;
  height: 50px;
  background-color: rgba(255, 255, 255, 0.85);
}
.bio .btn {
  margin-top: 45%;
  position: relative;
}
h1 {
  font-weight: 800;
  font-family: Roboto;
}
@media (min-width: 1200px) {
  .card-columns {
    -webkit-column-count: 4;
    column-count: 4;
    -webkit-column-gap: 1.25rem;
    column-gap: 1.25rem;
  }
  .card-columns .card {
    display: inline-block;
    width: 100%;
  }
}
.bio {
  transition: 0.5s ease;
  opacity: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  height: 100%;
  width: 100%;
  background-color: white;
  transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
}
.bio p {
  padding: 10px;
}
.jcard:hover .bio {
  opacity: 0.75;
}
.h-100 {
  height: 100%;
}
.card {
  border: none;
}
@media (min-width: 992px) {
  .container {
    max-width: 1500px;
  }
}
/* .card-body {
  position: absolute;
  bottom: 0px;
  color: white;
  background-color: rgba(255, 255, 255, 0.5);
  width: 100%;
} */
.card-title {
  margin-bottom: 0px;
}
p {
  margin-bottom: 0px;
}
.card-img-top {
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
  height: 100%;
}
@media screen and (max-device-width: 992px) {
  h1 {
    font-size: 1.2em;
    font-weight: 600;
    margin: 0 auto;
    margin-top: 20px;
  }
  .header {
    height: 12%;
  }
  .scroll-toggle {
    background-image: url(../assets/scroll.svg);
    border: 0;
    background-repeat: no-repeat;
    color: transparent;
    width: 25%;
    float: left;
    margin-top: 20px;
  }
  .btn-outline-primary:hover, .btn-outline-primary:not([disabled]):not(.disabled):active, .btn-outline-primary:not([disabled]):not(.disabled).active, .show > .btn-outline-primary.dropdown-toggle {
    background-color: transparent;
    color: transparent;
  }
  .signinlink {
    font-size: 0.75em;
    margin: 0 auto;
    display: inline;
    margin-top: 20px;
  }
  .checkin-desktop {
    display: none;
  }
  .checkin-mobile {
    display: inline-block;
    font-weight:  700;
  }

}

</style>

<script>
let fps = 100
let speedFactor = 0.0005
let minDelta = 0.5
let autoScrollSpeed = 10
let autoScrollTimer, restartTimer
let isScrolling = false
let prevPos = 0
let currentPos = 0
let currentTime, prevTime, timeDiff

function handleManualScroll (vm) {
  console.log(vm.isScrolling)
  vm.isScrolling = !vm.isScrolling
  currentPos = window.scrollY || window.pageYOffset
  clearInterval(autoScrollTimer)
  if (restartTimer) {
    clearTimeout(restartTimer)
  }
  restartTimer = setTimeout(() => {
    prevTime = null
  }, 50)
}

function setAutoScroll (newValue) {
  if (newValue) {
    autoScrollSpeed = speedFactor * newValue
  }
  if (autoScrollTimer) {
    clearInterval(autoScrollTimer)
  }
  autoScrollTimer = setInterval(function () {
    currentTime = Date.now()
    if (prevTime) {
      if (!isScrolling) {
        timeDiff = currentTime - prevTime
        currentPos += autoScrollSpeed * timeDiff
        if (Math.abs(currentPos - prevPos) >= minDelta) {
          isScrolling = true
          window.scrollTo(0, currentPos)
          isScrolling = false
          prevPos = currentPos
          prevTime = currentTime
        }
      }
    } else {
      prevTime = currentTime
    }
  }, 1000 / fps)
}

export default {
  methods: {
    scroll () {
      window.addEventListener('scroll', function (e) {
        currentPos = window.scrollY || window.pageYOffset
      })

      window.addEventListener('wheel', handleManualScroll)
      window.addEventListener('touchmove', handleManualScroll)

      setAutoScroll(20)
    }
  },
  created () {
    this.scroll()
  },
  data: function () {
    return {
      isScrolling: false,
      people: [
        {
          name: 'Kyle Minshall',
          bio: 'Hi! My name is Kyle Minshall. I am a second year Computer Science student at the University of California, Riverside. I have been programming for almost eight years across a wide variety of languages including PHP and Symfony, Ruby on Rails, Python, C++, Java, Swift, Objective-C, and JavaScript.',
          website: 'http://kyleminshall.com/',
          image: 'kyle.jpg'
        },
        {
          name: 'Brian Crites',
          bio: 'Graduate student spending most of my time doing research and teaching undergraduates. I do K-12 CS outreach instead of sleeping.',
          website: 'https://www.linkedin.com/in/brrcrites/',
          image: 'brian.jpg'
        },
        {
          name: 'Karen Kong',
          bio: 'My name is Karen Kong and I\'m an undergraduate Computer Science student at the University of California, Riverside. My high school\'s AP Computer Science class ignited my passion for solving challenges and expressing creativity through programming. From then on, my aspirations included earning a degree in computer science and working in the industry. To me, there\'s no greater satisfaction than designing, developing, and debugging a piece of code that is then placed into action in a larger project. In my spare time, you\'ll find me crafting planar graph algorithms and implementing system updates in the research lab and exploring Android application and game development.',
          website: 'https://kkong006.github.io/',
          image: 'karen.jpg'
        },
        {
          name: 'Aaroh Mankad',
          bio:
            'I love doing Web Development! Particular interests include but are not limited to: React, React Native, GraphQL, Neo4j, etc. Also love contributing to open source projects!',
          website: 'http://aarohmankad.com',
          image: 'aaroh.jpg'
        },
        {
          name: 'Daniel Stinson-Diess',
          bio: 'My name is Daniel Stinson-Diess and I\'m a 2nd year computer science major at UC Riverside. Academically, I\'m interested in the fields of computer security and pure mathematics. I\'m getting started with web and app development hoping to build some awesome projects!',
          website: 'http://dstindiess.net/',
          image: 'daniel.jpg'
        },
        {
          name: 'Henry Doan',
          bio: 'Senior with a cool start up',
          website: '#',
          image: 'henry.jpg'
        },
        {
          name: 'Maaz Mohamedy',
          bio: 'ACM Marketing Chair',
          website: '#',
          image: 'maaz.jpg'
        },
        {
          name: 'Neal Goyal',
          bio: 'I am currently a sophomore at the University of California, Riverside. I am majoring in computer science. I am interested in expanding my experience by seeking internships and research opportunities.',
          website: '#',
          image: 'neal.jpg'
        },
        {
          name: 'Gustavo Correa',
          bio: 'What’s happening friends! My name is Gustavo Correa and I’m going into my third year at UCR for Computer Engineering.',
          website: 'https://www.linkedin.com/in/correagustavo/',
          image: 'gustavo.jpg'
        },
        {
          name: 'Mario Salazar',
          bio: 'As a United States Marine I have learned, instilled, and developed many leadership traits and principles that will help me throughout my career. The most significant leadership principle that has positively influenced my life is to know myself and seek self improvement. This is a principle that I life by as I strive to continuously improve in all aspects of life.',
          website: 'https://www.linkedin.com/in/mariozar/',
          image: 'mario.jpg'
        },
        {
          name: 'Sid Sharma',
          bio: 'Hello, friends!',
          website: 'http://siddhanthsharma.org/',
          image: 'sid.jpg'
        },
        {
          name: 'Patrick Le',
          bio:
            'I enjoy swimming',
          website: '#',
          image: 'patrick.jpg'
        },
        {
          name: 'Zach Zimmerman',
          bio: 'Hi everyone! I’m Zach and I’ll be serving as the graduate student advisor for ACM in 2017-2018. I am a computer science PhD student and am going into my third year of the program here under my advisors Dr. Brisk and Dr. Keogh. At UCR, my research focus is on high performance computing and how we can utilize different sources of acceleration (GPUs, FPGAs, TPUs, etc.) to accelerate data mining and machine learning tasks. In particular, I am interested in time series data; examples of this are EEG and seismology readings.',
          website: 'https://www.linkedin.com/in/zpzim/',
          image: 'zach.jpg'
        },
        {
          name: 'Henry Doan',
          bio: 'Senior with a cool start up',
          website: 'https://www.linkedin.com/in/henry-doan-6b523165/',
          image: 'henry.jpg'
        },
        {
          name: 'Valerie Chiou',
          bio: 'Likes Chipotle',
          website: 'https://www.linkedin.com/in/valerie-chiou/',
          image: 'valerie.jpg'
        },
        {
          name: 'Katie Fukuda',
          bio: 'I have previously worked at Blizzard Entertainment for two summer internship opportunities and became really inspired to work with computers after my experience. Now, I am a sophomore at UC Riverside studying Computer Science, seeking real-world opportunities in the computing field.',
          website: 'https://www.linkedin.com/in/katie-fukuda/',
          image: 'katie.jpg'
        },
        {
          name: 'Daniel Lui',
          bio: 'MATLAB > All other languages',
          website: 'https://github.com/mintypaladin/',
          image: 'daniel_lui.jpg'
        }
      ]
    }
  }
}
</script>
