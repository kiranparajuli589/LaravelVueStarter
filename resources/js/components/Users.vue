<template>
    <div class="container">
        <div class="card mt-5">
            <div class="card-header">
                <h3 class="card-title">User Table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" data-toggle="modal" data-target="#addNewUserModal">Add New
                        <i class="fas fa-user-plus-fw"></i>
                    </button>
<!--                    <div class="input-group input-group-sm" style="width: 150px;">-->
<!--                        <input type="text" name="table_search" class="form-control float-right" placeholder="Search">-->

<!--                        <div class="input-group-append">-->
<!--                            <button type="submit" class="btn btn-default"><i class="fas fa-search"></i></button>-->
<!--                        </div>-->
<!--                    </div>-->
                </div>
            </div>
            <!-- /.card-header -->
            <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                    <thead>
                    <tr>
                        <th>ID</th>
                        <th>User</th>
                        <th>Email</th>
                        <th>Type</th>
                        <th>Registered At</th>
                        <th>Modify</th>
                    </tr>
                    </thead>
                    <tbody>

                    <tr v-for="user in users" :key="user.id">
                        <td>{{user.id}}</td>
                        <td>{{user.name}}</td>
                        <td>{{user.email}}</td>
                        <td>{{user.type}}</td>
                        <td>{{user.created_at}}</td>
                        <td>
                            <a class="mr-3" href="#">
                                <i class="fa fa-edit"></i>
                            </a>
                            <a href="#">
                                <i class="red fa fa-trash"></i>
                            </a>
                        </td>
                    </tr>

                    </tbody>
                </table>
            </div>
            <!-- /.card-body -->
        </div>

        <!-- Modal -->
        <div class="modal fade" id="addNewUserModal" tabindex="-1" role="dialog" aria-labelledby="addNewUserModal" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered " role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addNewUserLabel">Add New User</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="createUser">
                    <div class="modal-body">
                        <div class="form-group">
                            <input type="text" v-model="form.name"
                                   :class="{ 'is-invalid': form.errors.has('name') }"
                                   class="form-control" placeholder="Name">
                            <has-error :form="form" field="name"></has-error>
                        </div>
                        <div class="form-group">
                            <input type="email" v-model="form.email"
                                   :class="{ 'is-invalid': form.errors.has('email') }"
                                   class="form-control" placeholder="Email">
                            <has-error :form="form" field="email"></has-error>
                        </div>
                        <div class="form-group">
                            <textarea v-model="form.bio"
                                   :class="{ 'is-invalid': form.errors.has('bio') }"
                                      class="form-control" placeholder="Short bio for user (Optional)"></textarea>
                            <has-error :form="form" field="bio"></has-error>
                        </div>
                        <div class="form-group">
                            <select name="type" id="type" class="form-control"
                                    v-model="form.type" :class="{'is-invalid': form.errors.has('type')}">
                                <option value="">Select User Role</option>
                                <option value="admin">Admin</option>
                                <option value="user">Standard User</option>
                                <option value="author">Author</option>
                            </select>
                            <has-error :form="form" field="type"></has-error>
                        </div>
                        <div class="form-group">
                            <input type="password" v-model="form.password"
                                   class="form-control" :class="{'is-invalid': form.errors.has('password')}"
                                   name="password">
                            <has-error :form="form" field="password"></has-error>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Create</button>
                    </div>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                users : {},
                form: new Form({
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },
        methods: {
          loadUsers(){
              axios.get('api/user').then(({ data }) => (this.users = data.data) );
          },
          createUser(){
            this.form.post('api/user')
          }
        },
        mounted() {
            console.log('Component mounted.');
            this.loadUsers();
        }
    }
</script>
