<template>
  <b-container class="h-100 mt-5">
    <div class="text-center header row">
      <div class="col-md-1">
        <button @click="scroll(isScrolling); isScrolling = !isScrolling" class="scroll-toggle btn btn-sm btn-outline-primary">Turn On Scroll</button>
      </div>
      <div class="col-md-7">
        <h1>PEOPLE OF ACM</h1>
      </div>
      <div class="col-md-2 signinlink">
        <a href="http://acmucr.org/checkin">acmucr.org/checkin</a>
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
</style>

<script>
let fps = 100
let speedFactor = 0.001
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
          name: 'Aaroh Mankad',
          bio:
            'I love doing Web Development! Particular interests include but are not limited to: React, React Native, GraphQL, Neo4j, etc. Also love contributing to open source projects!',
          website: 'http://aarohmankad.com',
          image: 'aaroh.jpg'
        },
        {
          name: 'Brian Crites',
          bio: 'Pretty cool programmer Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus. Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus. ',
          website: '#',
          image: 'brian.jpg'
        },
        {
          name: 'Daniel Stinson-Diess',
          bio: 'Security nut crack Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus.  Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus. Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus.',
          website: '#',
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
          bio: 'Likes to skate around to the kitchen Nulla lacinia, ex sit amet semper euismod, lorem elit condimentum justo, vel fermentum leo mauris at magna. Nunc nunc diam, vestibulum id lobortis eget, finibus vel quam. Proin eget tincidunt risus, ut cursus risus. Ut auctor augue ac metus rhoncus, a semper sem laoreet. Aenean non hendrerit dui, in convallis sapien. In in ex et ex convallis pulvinar nec et magna. Maecenas dui nunc, ornare sed felis non, mattis laoreet purus. ',
          website: '#',
          image: 'maaz.jpg'
        },
        {
          name: 'Neal Goyal',
          bio: 'Nice guy',
          website: '#',
          image: 'neal.jpg'
        },
        {
          name: 'Sid Sharma',
          bio: 'Betaaaaaaaa',
          website: '#',
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
          bio: 'Whoaaaaaaaa',
          website: '#',
          image: 'zach.jpg'
        },
        {
          name: 'Henry Doan',
          bio: 'Senior with a cool start up',
          website: '#',
          image: 'henry.jpg'
        },
        {
          name: 'Katie Fukuda',
          bio: 'Likes to play games',
          website: '#',
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
