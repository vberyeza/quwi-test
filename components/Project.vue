<template>
  <li class="card" @click="onProjectClick(projectId)">
    <img :src="logoUrl" class="logo">
    <span class="name">{{ name }}</span>
    <span :class="{ status: true, 'status--active': isActive }">
      {{ isActive ? 'Active' : 'Inactive' }}
    </span>
    <div class="time">
      <p class="time-row">
        time this week <span>{{ spentTimeWeek | formattedTime }}</span>
      </p>
      <p>this month <span>{{ spentTimeMonth | formattedTime }}</span></p>
      <p>total <span>{{ spentTimeAll | formattedTime }}</span></p>
    </div>
  </li>
</template>

<script>
export default {
  name: 'Project',
  filters: {
    formattedTime (val) {
      const pad = num => (num > 9 ? num : `0${num}`)
      const date = new Date(val)
      const hours = date.getUTCHours()
      const minutes = date.getMinutes()
      const seconds = date.getSeconds()
      return `${pad(hours)}:${pad(minutes)}:${pad(seconds)}`
    }
  },
  props: {
    projectId: {
      type: Number,
      required: true
    },
    spentTimeWeek: {
      type: Number,
      required: true
    },
    spentTimeMonth: {
      type: Number,
      required: true
    },
    spentTimeAll: {
      type: Number,
      required: true
    },
    isActive: {
      type: Boolean,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    logoUrl: {
      type: String,
      required: true
    }
  },
  methods: {
    onProjectClick (id) {
      this.$router.push({ name: 'projects-id', params: { id } })
    }
  }
}
</script>

<style lang="scss" scoped>
.card {
  max-height: 100px;
  height: 100px;
  border-radius: 16px;
  border: 2px solid #eee;
  background-color: #fff;
  margin-top: 16px;
  display: flex;
  align-items: center;
  padding: 16px;

  &:hover {
    cursor: pointer;
  }
  &:last-child {
    margin-bottom: 16px;
  }
}

.logo {
  max-height: 60px;
  max-width: 60px;
  object-fit: cover;
  margin-right: 16px;
}

.name {
  max-width: 150px;
}

.status {
  font-size: 1rem;
  color: #7d0000;
  font-weight: bold;
  margin: 0 16px;

  &--active {
     color: #377d36;
  }
}

.time {
  & p {
    display: flex;
    justify-content: space-between;
  }
  & span {
    font-weight: bold;
    margin-left: 16px;
  }
}

</style>
