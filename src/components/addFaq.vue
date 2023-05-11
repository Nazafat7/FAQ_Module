<template>
 <div class="container">
    <div class="row">
        <div class="col">
            <!-- data-bs-toggle="modal" data-bs-target="#add_faqModel" -->
            <button type="button" class="btn btn-sm btn-outline-primary px-4 float-end" @click="showModal()">Add FAQ</button>
        </div>
    </div>
 </div>
 <!-- Modal -->
<div class="modal fade" id="add_faqModel" tabindex="-1" aria-labelledby="add_faqModelLabel" aria-hidden="true" ref="myModal">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="add_faqModelLabel">Add FAQ</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form class="was-validated" @submit.prevent="submitData">
            <div class="form-control mb-3">
                <label for="validationQuestion" class="form-label"> Question</label>
                <!-- {{ faqData.faqQuestion }} -->
                <textarea class="form-control is-invalid"  id="validationQuestion" rows="3" v-model.trim="faqData.faqQuestion" placeholder="Required Question" required ref="descInput"></textarea>
            </div>
            <div class="form-control mb-3">
                <!-- {{ faqData.faqAnswer }} -->
                <label ffor="validationAnswer" class="form-label">Answer</label>
                <textarea class="form-control is-invalid"  id="validationAnswer" rows="3" v-model.trim="faqData.faqAnswer" placeholder="Required Answer" required ref="descInput"></textarea>
            </div>
            <div>
                <button type="submit" class="btn btn-sm btn-outline-primary px-4 float-end">Submit</button>
            </div>
        </form>
      </div>
    </div>
  </div>
</div>
<!-- Modal Ends -->
</template>
<script>
import { Modal } from 'bootstrap';
export default {
inject: ['addFAQ'],
name: 'addFaq',
props: {
  
},
data(){
    return{
        lastId:0,
        faqData:{
            id:0,
            faqQuestion:'',
            faqAnswer:''
        },
        myModal: false,
        

    }
},
mounted(){
  let FAQData = localStorage.getItem('FAQData')
      if(FAQData != null){
        let FAQParse = JSON.parse(FAQData.toString());
        console.log("MountedJSONParse",FAQParse);
        this.lastId=FAQParse.length;
      }
},
computed: {
  newId() {
   
        return this.lastId + 1
    
  }
},

methods:{
    submitData() {
        this.faqData.id = this.newId
      const enteredFAQ = this.faqData;
      this.addFAQ(enteredFAQ);
      this.lastId = this.newId
      this.myModal.hide();
      
    },
    showModal(){
        this.myModal = new Modal(document.getElementById('add_faqModel'), {})
        this.myModal.show()
        this.faqData={}
    }
}
}
</script>
<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>