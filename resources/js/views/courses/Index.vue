<template>
    <div class="body-content">
        <b-row>
            <b-col offset-lg="1" col lg="10">
                <h1>Courses</h1>

                <b-table-simple responsive>
                    <b-tr>
                        <b-th>Title</b-th>
                        <b-th>Code</b-th>
                        <b-th>Points</b-th>
                        <b-th>Level</b-th>
                        <b-th>Actions</b-th>
                    </b-tr>

                    <b-tr v-for="course in courses" :key="course.id">
                        <b-td>{{ course.title }}</b-td>
                        <b-td>{{ course.code }}</b-td>
                        <b-td>{{ course.points }}</b-td>
                        <b-td>{{ course.level }}</b-td>
                        <b-td>
                            <router-link :to="`/dashboard/courses/${course.id}`">
                                <b-button variant="outline-secondary">
                                    <jam-eye/>View
                                </b-button>
                            </router-link>
                            <router-link :to="`/dashboard/courses/edit/${course.id}`">
                                <b-button variant="outline-secondary">
                                    <jam-pencil/>Edit
                                </b-button>
                            </router-link>
                            <b-button variant="outline-secondary" @click="deleteCourse(course.id)">
                                <jam-trash-alt/>Delete
                            </b-button>
                        </b-td>
                    </b-tr>
                </b-table-simple>
            </b-col>
        </b-row>
    </div>
</template>

<script>
    export default {
        name: 'coursesIndex',
        data() {
            return {
                courses: [],
                loggedIn: false
            }
        },
        mounted() {
            if (localStorage.getItem('token') !== null) {
                // console.log("User logged in.");
                this.loggedIn = true;
            } else {
                // console.log("User not logged in.");
                this.loggedIn = false;
                this.$router.push('/');
            }
            this.getCourses();
        },
        methods: {
            getCourses() {
                let app = this;
                let token = localStorage.getItem('token');
                axios.get('/api/courses', {
                    headers: { Authorization: "Bearer " + token}
                })
                .then(function (response) {
                    console.log(response.data);
                    app.courses = response.data.data;
                })
                .catch(function (error) {
                    console.log(error);
                })
            },
            deleteCourse(id) {
                let app = this;
                let token = localStorage.getItem('token');
                axios.delete(`/api/courses/${id}`, {
                    headers: { Authorization: "Bearer " + token }
                })
                .then(function (response) {
                    console.log(response);
                    app.getCourses();
                });
            }
        }
    }
</script>