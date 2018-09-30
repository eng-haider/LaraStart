
<template>

   <div class="container">

   <div class="row">

       <div class="col-md-12">

         <div class="box">

           <div class="box-header">

             <h3 class="box-title">Responsive Hover Table</h3>


      


             <div class="box-tools">

                 <button type="button" class="btn btn-success"  style="float:right"  @click="newModal">

                       Add New <i class="fas fa-user-plus fa-fe"></i>

                       </button>

                 </div>



               <div class="input-group input-group-sm" style="width: 150px;">

                 <input type="text" name="table_search" class="form-control pull-right" placeholder="Search">


                 <div class="input-group-btn">

                   <button type="submit" class="btn btn-default"><i class="fa fa-search"></i></button>

                 </div>

               </div>

             </div>


                    <div class="card-title ">

         

                 


           </div>

           <!-- /.box-header -->

           <div class="card-body table-responsive p-0">

             <table class="table table-hover">

               <tbody>


               <tr>

                   <th>ID</th>

                   <th>Name</th>

                   <th>Email</th>

                   <th>Type</th>

                  <th>Register At</th>

                   <th>Modify</th>

               </tr>


               <tr v-for="user in users" :key="user.id">

                 <td>{{user.id}}</td>

                 <td>{{user.name}}</td>

                 <td>{{user.email}}</td>

                 <td>{{user.type | upText}}</td>

                 <td>{{user.created_at|myDate}}</td>

                 <td><span class="label label-success">Approved</span></td>

                 <td>

                     <a href="#" @click="editModal(user)">

                         <i class="fa fa-edit"></i>

                     </a>

                       /

                      <a href="#" @click="deleteUser(user.id)">

                         <i class="fa fa-trash red"></i>

                     </a>


                 </td>

               </tr>

             

             </tbody></table>

           </div>

           <!-- /.box-body -->

         </div>

         <!-- /.box -->

       </div>

     </div>


     <!-- Modal -->

       <div class="modal fade" id="adduser" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">

       <div class="modal-dialog" role="document">

           <div class="modal-content">

           <div class="modal-header">

               <h5 class="modal-title" v-show="!editmode" id="exampleModalLabel">Add User</h5>

               <h5 class="modal-title" v-show="editmode" id="exampleModalLabel">Update User's Info</h5>

               <button type="button" class="close" data-dismiss="modal" aria-label="Close">

               <span aria-hidden="true">&times;</span>

               </button>

           </div>
           
 <form   @submit.prevent="editmode ? updateUser() : createUser()" >

           <div class="modal-body">


              
                   <div class="form-group">            

                       <input v-model="form.name" type="text" name="name"

                            placeholder="Name"

                           class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">

                       <has-error :form="form" field="name"></has-error>

                   </div>


                     <div class="form-group">            

                       <input v-model="form.email" type="text" name="email"

                            placeholder="Email"

                           class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">

                       <has-error :form="form" field="email"></has-error>

                   </div>



                      <div class="form-group">            

                       <textarea v-model="form.bio"  name="bio"

                            placeholder="Enter Short chart of Bio"

                           class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>

                       <has-error :form="form" field="bio"></has-error>

                   </div>



                   


                   <div class="form-group">            

                      <select name="type" v-model="form.type" id ="type" class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">

                          <option value="">Select User Role</option>

                          <option value="admin">Admin</option>

                          <option value="standarad">Standard</option>

                          <option value="Auther">Author</option>

                      </select>
                    <has-error :form="form" field="type"></has-error>
                   </div>


                   





                     <div class="form-group">            

                       <input v-model="form.password" type="password" name="password"

                            placeholder="Password"

                           class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">

                       <has-error :form="form" field="password"></has-error>

                   </div>


           </div>

           <div class="modal-footer">

               <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>

               <button v-show="!editmode" type="submit" class="btn btn-primary">Create</button>

               <button v-show="editmode"  type="submit" class="btn btn-success">Update</button>


           </div>
 </form>
           </div>

          

       </div>

       </div>

       <!-- End Modal -->



   </div>

</template>


   <script>

       export default {


           data()

           {

               return {
                editmode:false,
                   users:{},
                   form:new Form({

                          

                           name:'',

                           email:'',

                           password:'',

                           type:'',

                           bio:'',

                           photo:'',


                   })

               }

           },


           methods:{


               updateUser(id){   
                   this.$Progress.start(); 
                   this.form.put('api/users/'+this.form.id)
                   .then(()=>{
                       //success
                   })
                   .catch(()=>{

                       this.$Progress.start(); 
                   });

                   // console.log('Editing the Data');

               },

                editModal(user){
                     this.editmode=true;
                    this.form.reset();
                    $('#adduser').modal('show');
                    this.form.fill(user);
               },

                 newModal(){
                    this.editmode=false;
                    this.form.reset();
                    $('#adduser').modal('show')
               },

               deleteUser(id)

               {
                    swal({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                    }).then((result) => {

                    //send request to the server
                  
                          if (result.value) {
                                this.form.delete('api/users/'+id).then(()=>{
                            swal(
                            'Deleted!',
                            'Your file has been deleted.',
                            'success'
                            )
                            Fire.$emit('AfterCreate');
                         

                        } ).catch(()=>{
                                swal(
                                'Faild!',
                                'There Was Sonthing Wrong',
                                'Warning'
                                )

                        });
                     }

                    })
                 
               },


               loadUsers() {
                   axios.get("api/users").then(({data})=>(this.users=data.data));
               },

               createUser()

               {

                    this.$Progress.start();                                  
                    this.form.post('api/users')

                    .then(()=>{
                    Fire.$emit('AfterCreate');
                    $('#adduser').modal('hide')
                    toast({
                    type: 'success',
                    title: 'User Created in successfully '
                    })
                    this.$Progress.finish();
                    })

                    .catch(()=>{

                    })


                  
               }
           },

           created() {
                this.loadUsers();
                Fire.$on('AfterCreate',()=>{
                    this.loadUsers();
                });
                //setInterval(()=> this.loadUsers(),3000);
                
           }

       }
       

   </script>


  