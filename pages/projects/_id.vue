<template>
  <div class="container">
    <app-header :title="`Project: ${project.name}`" />
    <div class="subheader">
      <router-link type="button" to="/">
        Back
      </router-link>
    </div>
    <div v-if="project" class="card">
      <h2>Name</h2>
      <form @submit.prevent="updateProject">
        <input v-model="name" class="field" type="text">
        <styled-button type="submit" text="ok" />
      </form>
    </div>
    <snackbar :message="message" :error="error" :success="success" />
  </div>
</template>

<script>
import AppHeader from '~/components/AppHeader'
import Snackbar from '~/components/Snackbar'
import StyledButton from '~/components/StyledButton'

export default ({
  middleware: 'authenticated',
  components: {
    AppHeader,
    Snackbar,
    StyledButton
  },
  async asyncData ({ params, route, $axios }) {
    const { data } = await $axios.get(`/projects-manage/${params.id}`)
    return {
      project: data.project,
      name: data.project.name,
      message: null,
      error: false,
      success: false
    }
  },
  methods: {
    async updateProject () {
      try {
        const { params } = this.$route
        const formData = new FormData()
        formData.append('name', this.name)
        const { data: res } = await this.$axios.post(
          `/projects-manage/update?id=${params.id}`,
          formData,
          { headers: { 'Content-Type': 'multipart/form-data' } }
        )
        this.showSnackSuccess()
        this.project = res.project
      } catch (err) {
        this.showSnackError()
      }
    },
    showSnackError () {
      this.error = true
      this.message = 'Error updating project name'
      this.resetSnack()
    },
    showSnackSuccess () {
      this.success = true
      this.message = 'Project name updated'
      this.resetSnack()
    },
    resetSnack () {
      setTimeout(() => {
        this.error = false
        this.success = false
        this.message = null
      }, 3000)
    }
  }
})
</script>

<style lang="scss" scoped>
.container {
  background-color: #f4f4f4;
  padding-top: 64px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-height: 100vh;
}
.subheader {
  background-color: #c1c8d2;
  padding: 16px;
  width: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  & a {
    background-color: #cecece;
    color: #151515;
    text-transform: uppercase;
    padding: 16px;
    border-radius: 4px;
    border: 1px solid #bebfbf;
    font-weight: bold;
    font-size: 1rem;
    text-decoration: none;

    &:hover {
      cursor: pointer;
    }
  }
}
.card {
  padding: 64px;
  background-color: #fff;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #bebfbf;
  border-radius: 4px;
  margin: 16px auto;

  & h2 {
    margin-right: 64px;
  }
}
.field {
  min-width: 400px;
  background-color: #fff;
  border: 1px solid #cccccc;
  border-radius: 4px;
  padding: 16px 8px;
  font-size: 1rem;
  color: #151515;
  &::placeholder {
    color: #cccccc;
  }
}
</style>
