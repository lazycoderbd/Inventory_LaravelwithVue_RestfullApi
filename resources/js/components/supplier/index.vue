 <template>
      <div>
        <!-- Breadcrumbs-->
        <ol class="breadcrumb">
          <li class="breadcrumb-item">
            <a href="#">Dashboard</a>
          </li>
          <li class="breadcrumb-item active">All Supplier</li>
        </ol>
        <!-- Icon Cards-->
       <div class="row card container">
          <div class="card-header">
            <i class="fas fa-chart-area"></i>
            Supplier Insert 
            <router-link to="/store-supplier" class="btn btn-sm btn-info" id="add_new"> Add New</router-link>
          </div>
          <div class="card-body">
          
          <div class="card-body">
              <div class="table-responsive">
                <label>Search</label>
                <input type="text" v-model="searchItem"  class="form-control" style="width:200px; "><br>
                <table class="table table-bordered" id="" width="100%" cellspacing="0">

                  <thead>
                    <tr>
                      <th>Name</th>
                      <th>Photo</th>
                      <th>Phone</th>
                      <th>shopname</th>
                      <th>email</th>
                      <th>Action</th>
                    </tr>
                  </thead>
                
                  <tbody>

                    <tr v-for="supplier in filterSearch" :key="supplier.id">
                      <td>{{supplier.name}}</td>
                      <td><img :src="supplier.photo" id="em_photo"></td>
                      <td>{{supplier.phone}}</td>
                      <td>{{supplier.shopname}}</td>
                      <td>{{supplier.email}}</td>
                      <td>
                        <router-link :to="{name: 'edit-supplier',params:{id:supplier.id}}" class="btn btn-sm btn-info">Edit</router-link>
                        <a @click="deleteSupplier(supplier.id)" class="btn btn-sm btn-danger">Delete</a>
                      </td>
                    </tr>
        
                   </tbody>
                </table>
              </div>
            </div>
            <div class="card-footer small text-muted">Updated yesterday at 11:59 PM</div>
          </div>
       </div>
   </div>
</template>

<script>

    export default {
    mounted(){
            if (!User.loggedIn()) {
               this.$router.push({ name:'/' })
            } 
        },
        data()
        {
            return{
                suppliers:[],
                searchItem:''
            }
            
        },
        computed:
        {
            filterSearch()
            {
                return this.suppliers.filter(supplier=>{
                    return supplier.phone.match(this.searchItem);
                   
                })
            }
        },
        methods:
        {
            allSupplier()
                {
                    axios.get('/api/supplier/')
                    .then(({data})=> (this.suppliers=data))
                    .catch()
                },
                deleteSupplier(id)
                {
                    Swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                    }).then((result) => {
                    if (result.value) {
                        axios.delete('/api/supplier/'+id)
                        .then(()=>{
                            this.suppliers=this.suppliers.filter(supplier=>{
                                return supplier.id !=id
                            })
                        })
                        .catch(console.log('error'))
                        Swal.fire(
                        'Deleted!',
                        'Your file has been deleted.',
                        'success'
                        )
                    }
                    })
                }
                
            
        },
        created()
        {
            this.allSupplier();
        }
        	
    }



  
</script>

<style>
#add_new{
    float:right;
}
	
#em_photo{
    height:40px;
    width:40px;
}

</style>
