
<style>
.widget-user-header
{
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}
</style>

<template>
    <div class="container">
        <div class="row mt-3">
            <div class="col-md-12">
                <div class="box box-widget widget-user">
            <!-- Add the bg color to the header using any of the bg-* classes -->
            <div class="widget-user-header bg-black" style="background: url('./img/user-cover.jpg') center center; background-repeat: no-repeat;background-size: cover;">
              <h3 class="widget-user-username">Elizabeth Pierce</h3>
              <h5 class="widget-user-desc">Web Designer</h5>
            </div>
            <div class="widget-user-image">
              <img class="img-circle" src="" alt="User Avatar">
            </div>
            <div class="box-footer">
              <div class="row">
                <div class="col-sm-4 border-right">
                  <div class="description-block">
                    <h5 class="description-header">3,200</h5>
                    <span class="description-text">SALES</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
                <div class="col-sm-4 border-right">
                  <div class="description-block">
                    <h5 class="description-header">13,000</h5>
                    <span class="description-text">FOLLOWERS</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
                <div class="col-sm-4">
                  <div class="description-block">
                    <h5 class="description-header">35</h5>
                    <span class="description-text">PRODUCTS</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
              </div>
              <!-- /.row -->
            </div>
          </div>
               
            </div>
        </div>

    

    <!--edite profile-->
<div class="nav-tabs-custom" style="border:3px solid #eee">
            <ul class="nav nav-tabs">
                 
                    <li class="active"> <a href="#activity"  class="btn btn-primary" data-toggle="tab" aria-expanded="true" style="margin:10px;">Activity</a></li>
                    <li class=""><a href="#settings" class="btn btn-primary" data-toggle="tab" aria-expanded="false" style="margin:10px;">Settings</a></li>

                 
            </ul>
            <div class="tab-content">
              <div class="tab-pane active" id="activity">
                <br>
                   <P>HERE ACTIVE profile</p>
              </div>
              <!-- /.tab-pane -->













              <div class="tab-pane" id="settings">
                <form class="form-horizontal mt-3">
                  <div class="form-group">
                    <label for="inputName" class="col-sm-2 control-label">Name</label>

                    <div class="col-sm-10 ">
                      <input type="text" v-model="form.name"  class="form-control" id="inputName" placeholder="Name">
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                    <div class="col-sm-10">
                      <input type="email" v-model="form.email"  class="form-control" id="inputEmail" placeholder="Email">
                    </div>
                  </div>
                

                  <div class="form-group">
                    <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                    <div class="col-sm-10">
                      <textarea v-model="form.bio" class="form-control" id="inputExperience" placeholder="Experience"></textarea>
                    </div>
                  </div>

                  <div class="form-group">
                      <label for="inputExperience" class="col-sm-2 control-label">Profile Picture </label>
                       <div class="col-sm-10">
                     <input type="file" id="exampleInputFile" class="form-control" @change="updateProfile" >

                    </div>
                </div>


                  <div class="form-group">
                    <label for="inputSkills" class="col-sm-2 control-label">Passport</label>

                    <div class="col-sm-10">
                      <input type="text" class="form-control" id="inputSkills" placeholder="Skills">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="col-sm-offset-2 col-sm-10">
                      <div class="checkbox">
                        <label>
                          <input type="checkbox"> I agree to the <a href="#">terms and conditions</a>
                        </label>
                      </div>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="col-sm-offset-2 col-sm-10">
                      <button @click.prevent="updateInfo" type="submit" class="btn btn-danger">Submit</button>
                    </div>
                  </div>
                </form>
              </div>
              <!-- /.tab-pane -->
            </div>
            <!-- /.tab-content -->
          </div>

    <!--End Edite Profile-->






     
    
    
    
    </div>
</template>


   <script>

       export default {

           data()

           {

               return {
        
                   form:new Form({

                          
                           id:'',

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

             updateInfo(){
               this.form.put('api/profile')
               .then(()=>{

               })

              .catch(()=>{

               })
        
        
             },

                loadUsers() {
                   axios.get("api/profile")
                   .then(({data})=>(this.form.fill(data)));
               },

               updateProfile(e)
               {
                 let file=e.target.files[0];
 
                 let reader=new FileReader();

                 

                 reader.onloadend=(file)=>{
                 this.form.photo=reader.result;
                 }

                 reader.readAsDataURL(file); 

               }



           },

              created() {
                this.loadUsers();
                Fire.$on('AfterCreate',()=>{
                    this.loadUsers();
                });
               
                
           }

       }
       

</script>
