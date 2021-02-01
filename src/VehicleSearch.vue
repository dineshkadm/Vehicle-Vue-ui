<template>
<h1>{{ error}}</h1>
<h1> Available Vehicles </h1>

            <h3>Choose Vehicle to get price</h3>
        
 <table class="table table-striped table-bordered" border="1">
            <thead>
                <tr>
                    <th></th>
                    <th>ID</th>
                    <th>Make</th>
                    <th>Registration</th>
                    <th>Category</th>
                    <th>Fuel Type</th>
                    <th>Model</th>
                  
                </tr>
            </thead>
            <tbody>
                <tr v-for="v in responseVehicleList" :key="v.id">

                     <td>
                        <input type="radio" name="picked" @change="onChange($event)" :value="v.id"/>
                              
                    </td>
                    <td>{{v.id}} </td>
                    <td>{{v.make}} </td>
                    <td>{{v.registration}}</td>
                    <td>{{v.category}}</td>
                    <td>{{v.fueltype}}</td>
                    <td>{{v.model}}</td>                    
                    
                       
                </tr>           
                
            </tbody>
</table> 
<br>
 <table>
    <tbody>
          
        <tr>
            <td>From Date (dd.mm.yy)</td>
            <td>To Date (dd.mm.yy)</td>
            <td>Price (£)</td>
        </tr>

        <tr>
        <td>
                     <input v-model="fromDate" id="fromDate" placeholder="23.01.22">
                    </td>                       
                    <td>
                        <input v-model="toDate" id="toDate"  placeholder="25.01.22">
                    </td>  
                    <td>{{totalCost}}</td>     
                    <td>
                        <button type="button" id = "bt" @click = "getIPriceCall()">Get a Price!!</button>                        
                        </td>       
        </tr>
    </tbody>
</table> 

</template>
<script>
export default {
 
    // components: {        
    //     DatePicker
    // }, 
    data(){
        return{
            responseVehicleList: "", 
            error: ""     , 
            fromDate: "23.01.22",
            toDate: "25.01.22",
            totalCost: "",
            vehicleId: ""
           
        }
    },
    
    mounted ()  {

    this.getInventoryCall();
   
  },

  
    
    methods:{           

        async getInventoryCall() {
            const URL = "http://localhost:8081"
            try{
            const fetchResult = fetch(URL) 
                    
            const response = await fetchResult;
            const jsonData = await response.json();
            
            this.responseVehicleList=jsonData;
            console.log(jsonData);

            jsonData.forEach(obj => {
                    Object.entries(obj).forEach(([key, value]) => {
                        console.log(`${key} ${value}`);
                    });
                    console.log('-------------------');
                });

            }catch (err){
                this.error= err;
            }
        },
        
        onChange(event) {
              var data = event.target.value;
              this.vehicleId= data              
        },

        async getIPriceCall() {
                                   
              if (this.fromDate !=null && this.toDate !=null && this.vehicleId !=null)
              {          
                    
                const URL = "http://localhost:8081/calculate?vehicleId=" + this.vehicleId + "&fromDate=" + this.fromDate + "&toDate=" + this.toDate
                 try{
            const fetchResult = fetch(URL) 
                    
            const response =  await fetchResult;
            const jsonData =  await response.json();
            
            this.totalCost=jsonData.totalCost;
            console.log(jsonData);

            }catch (err){
                this.error= err;
            }
              }
        }
    }

}
</script>

<style lang="stylus"></style>