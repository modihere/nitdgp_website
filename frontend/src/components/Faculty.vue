<template>
  <div class="l3-fac">
    <sp-card containerclass="l3-card">
      <div slot="header">
        <p class="tile-title-text">
          <a class="white-text" :href="'/department/' + faculty[0].dept_short_code">
            FACULTY | {{ faculty[0].dept_short_code }}
          </a>
          <span v-if="!general" @click="general = true">
            - Show
          </span>
        </p>
      </div>
      <div class="container-fluid fac-sm">
        <div class="row">
          <div class="photo" :class="getClassImg()">
            <img :src="faculty[0].image" style="max-width: 100%; max-height: 100%">
            <p style="font-weight: bold">{{ faculty[0].name }}</p>
            <p>{{ faculty[0].designation }}</p>
            <i class="fa fa-mortar-board"></i><br>
            <span v-html="faculty[0].education" style="font-size: 80%"/>
            <p style="font-size: 80%">
              <strong v-if="faculty[0].joining_year==1959" >Joined in N/A<br></strong>
              <strong v-if="faculty[0].joining_year!=1959" >Joined in {{ faculty[0].joining_year }}<br></strong>
              <i class="fa fa-envelope"></i><br>
              {{ faculty[0].email }}
            </p>
          </div>
          <div class="downc tab-content" :class="getClassContent()">
            <button class="btn" type="button" @click="showNav($event)">
              <i class="fa fa-bars fa-2x"></i>
            </button>
            <div class="tab-pane fade show active big-list" id="li1" role="tabpanel">
              <h3 class="pane-title" align="left">Education</h3>
              <hr>
              <span v-if="'education' in faculty[0]" v-html="faculty[0].education"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>

            <div class="tab-pane fade big-list" id="li2" role="tabpanel">
              <h3 class="pane-title" align="left">Work Experiences</h3>
              <hr>
              <span v-if="'work_experience' in faculty[0]" v-html="faculty[0].work_experience"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>

            <div class="tab-pane fade big-list" id="li3" role="tabpanel">
              <h3 class="pane-title" align="left">Research Interest</h3>
              <hr>
              <span v-if="'research_interest' in faculty[0]" v-html="faculty[0].research_interest"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>

            <div class="tab-pane fade big-list" id="li4" role="tabpanel">
              <h3 class="pane-title" align="left">Projects</h3>
              <hr>
              <span v-if="'projects' in faculty[0]" v-html="faculty[0].projects"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>

            <div class="tab-pane fade big-list" id="li5" role="tabpanel">
              <h3 class="pane-title" align="left">Publication</h3>
              <hr>
              <ul class="nav md-pills nav-justified pills-secondary">
                <li class="nav-item">
                  <a class="nav-link active" data-toggle="tab" href="#p5l1" role="tab">Journal</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" data-toggle="tab" href="#p5l2" role="tab">Conference</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" data-toggle="tab" href="#p5l3" role="tab">Books</a>
                </li>
                <li class="nav-item">
                  <a class="nav-link" data-toggle="tab" href="#p5l4" role="tab">Patents</a>
                </li>
              </ul>
              <div class="tab-content">
                <div class="tab-pane fade in show active" id="p5l1" role="tabpanel">
                  <h3>Journals</h3>
                  <table-renderer :table="faculty[0].journals" :theader="['Citation','Journal','Year']"></table-renderer>
                </div>
                <div class="tab-pane fade page-type-links" id="p5l2" role="tabpanel">
                  <ul class="list-group list-gr">
                  <h3>Conferences</h3>
                  <table-renderer :table="faculty[0].conferences" :theader="['Citation','Location','Year']"></table-renderer>
                  </ul>
                </div>
                <div class="tab-pane fade in" id="p5l3" role="tabpanel">
                  <h3>Books</h3>
                  <ul>
                    <li v-for="book in faculty[0].books" >
                      <a v-if="book.file" :href="book.file" target="new">{{ book.title }}</a>
                      <a v-else :href="book.url" target="new">{{ book.title }}</a>
                    </li>
                  </ul>
                </div>
                <div class="tab-pane fade in" id="p5l4" role="tabpanel">
                  <h3>Patents</h3>
                    <ul>
                      <li v-for="patent in faculty[0].patents" >
                        <a v-if="patent.file" :href="patent.file" target="new">{{ patent.title }}</a>
                      </li>
                    </ul>
                </div>
              </div>
            </div>

            <div class="tab-pane fade big-list" id="li6" role="tabpanel">
              <h3 class="pane-title" align="left">Teachings</h3>
              <hr>
              <span v-if="'teachings' in faculty[0]" v-html="faculty[0].teachings"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>
            <div class="tab-pane fade big-list" id="li7" role="tabpanel">
              <h3 class="pane-title" align="left">List of students</h3>
              <hr>
              <div v-if="'students' in faculty[0]">
                <ul>
                  <li v-for="student in faculty[0].students">
                    <a :href="student.file" target="new">{{student.title}}</a>
                  </li>
                </ul>
              </div>
              <h4 v-else class="red-text">Not Available</h4>
            </div>
            <div class="tab-pane fade big-list" id="li8" role="tabpanel">
              <h3 class="pane-title" align="left">Awards and Recognitions</h3>
              <hr>
              <span v-if="'awards_and_recognition' in faculty[0]" v-html="faculty[0].awards_and_recognition"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>
            <div class="tab-pane fade big-list" id="li9" role="tabpanel">
              <h3 class="pane-title" align="left">Administrative Responsibilities</h3>
              <hr>
              <span v-if="'administrative_responsibilities' in faculty[0]" v-html="faculty[0].administrative_responsibilities"/>
              <h4 v-else class="red-text">Not Available</h4>
            </div>
            <div class="tab-pane fade big-list" id="li10" role="tabpanel">
              <h3 class="pane-title" align="left">Contact</h3>
              <hr>
              <span>Mobile : +91-{{faculty[0].mobile}}<br/>
                    Email : {{faculty[0].email}}<br/>
              </span>
            </div>
          </div>
          <div class="list-group"
            :class="extraNavClasses.concat([getClassNav()])"
            :style="{ top: popUpTop + 'px' }">
            <a class="dropdown-item active" :class="{ 'disabled': !('education' in faculty[0]) }" data-toggle="list" href="#li1" role="tab">Education</a>
            <a class="dropdown-item" :class="{ 'disabled': !('work_experience' in faculty[0]) }" data-toggle="list" href="#li2" role="tab">Work Experience</a>
            <a class="dropdown-item" :class="{ 'disabled': !('research_interest' in faculty[0]) }" data-toggle="list" href="#li3" role="tab">Research Interest</a>
            <a class="dropdown-item" :class="{ 'disabled': !('projects' in faculty[0]) }" data-toggle="list" href="#li4" role="tab">Projects</a>
            <a class="dropdown-item" data-toggle="list" href="#li5" role="tab">Publication</a>
            <a class="dropdown-item" :class="{ 'disabled': !('teachings' in faculty[0]) }" data-toggle="list" href="#li6" role="tab">Teachings</a>
            <a class="dropdown-item" :class="{ 'disabled': !('students' in faculty[0]) }" data-toggle="list" href="#li7" role="tab">Students</a>
            <a class="dropdown-item" :class="{ 'disabled': !('awards_and_recognition' in faculty[0]) }" data-toggle="list" href="#li8" role="tab">Awards and recognitions</a>
            <a class="dropdown-item" :class="{ 'disabled': !('administrative_responsibilities' in faculty[0]) }" data-toggle="list" href="#li9" role="tab">Administrative Responsibilities</a>
            <a class="dropdown-item" data-toggle="list" href="#li10" role="tab">Contact</a>
            <a class="dropdown-item" :href="genBackendURL('admin')" role="tab">Faculty Login</a>
          </div>
          <div v-show="nav" id="fade" class="black_overlay"></div>
        </div>
      </div>
    </sp-card>
  </div>
</template>
<script>
import axios from 'axios'
import { genBackendURL } from '@/common.js'
import TableRenderer from '@/components/TableRenderer'
import CardCollapse from '@/components/CardCollapse'
import SpCard from '@/components/SpCard'

export default {
  name: 'Faculty',
  data () {
    return {
      faculty: {},
      general: true,
      windowWidth: 1000,
      extraNavClasses: [],
      nav: false,
      popUpTop: 0,
      top: -1
    }
  },
  created () {
    axios.get(genBackendURL("faculty/" + this.$route.params.id))
         .then(response => {
           this.faculty = response.data.faculty
           console.log(this.faculty[0].mobile)
           for (var key in this.faculty) {
             if (this.faculty.hasOwnProperty(key) &&
                 Object.keys(this.faculty[key]).length === 0) {
               delete this.faculty[key];
             }
           }
           this.$emit('hideloader', true)
         })
         .catch(e => {
           console.log(e)
           window.location = '/NotAvailable'
         })
    window.addEventListener('resize', this.updateWidth)
    this.windowWidth = document.body.clientWidth
  },
  mounted () {
    Array.from(document.getElementsByClassName('dropdown-item')).forEach(el => {
      el.addEventListener('click', this.hideNav)
    })
  },
  methods: {
    genBackendURL: genBackendURL,
    hideNav () {
      this.nav = false
      this.extraNavClasses = []
      this.popUpTop = 0
    },
    showNav (event) {
      this.extraNavClasses = ['lift', 'div-block']
      this.nav = true
      this.popUpTop = event.clientY + this.top * 200
    },
    getClassImg () {
      let width = this.windowWidth
      if (width > 1180)
        return 'col-2'
      else if (width > 900)
        return 'col-2'
      else if (width > 600)
        return 'col-4'
      else
        return 'col-12'
    },
    getClassContent () {
      let width = this.windowWidth
      if (width > 1180)
        return 'col-8'
      else if (width > 900)
        return 'col-7'
      else if (width > 600) {
        this.top = -1
        return 'col-8'
      }
      else {
        this.top = 1
        return 'col-12'
      }
    },
    getClassNav () {
      let width = this.windowWidth
      if (width > 1180)
        return 'col-2'
      else if (width > 900)
        return 'col-3'
      else
        return 'div-none'
    },
    updateWidth () {
      this.windowWidth = document.body.clientWidth
    }
  },
  components: {
    TableRenderer,
    CardCollapse,
    SpCard
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateWidth);
  }
}
</script>

<style scoped>
    .list-group a, .list-group a:hover {
      white-space: pre-wrap;
    }
    .dropdown-item:hover{
      background-color: #001333!important;
      color: #fff!important;
    }
    .photo {
      text-align: center;
    }
    .downc {
      margin: 0 auto;
      padding: 10px;
    }
    .btn {
      background: #001333;
      width: 50px;
      padding: 10px;
      float: right;
      margin-left: auto;
      margin-bottom: 0px;
    }
    @media screen and (min-width: 909px) {
      .btn {
        display: none;
        height: 0px;
      }
    }
    .lift {
      position: absolute;
      left: 10%;
      width: 80%;
      background-color: white;
      z-index: 2;
      overflow: auto;
    }
    .black_overlay {
      position: absolute;
      top: 0%;
      left: 0%;
      bottom: 0%;
      width: 100%;
      height: 100%;
      z-index: 1;
      background-color: black;
      -moz-opacity: 0.8;
      opacity: .80;
      filter: alpha(opacity=80);
    }
</style>
