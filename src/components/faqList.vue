<template>
  <div class="container mt-3">

      <div class="row align-items-start">

        <div class="col">


              <div class="row d-flex justify-content-center align-items-center mb-3">

                <div class="col-md-6">

                  <div class="form">
                    <input type="text" v-model="searchTerm" class="form-control form-input" placeholder="Search Questions...">
                  </div>
                  
                </div>
                
              </div>

          <div class="card">
            
            <div class="card-header text-dark mb-4">    
                <div class="row">
                  <div class="col-md-12">
                    <addFaq/>
                  </div>
                </div>
            </div>

            <div class="card-body"> 
              <div v-if="filteredQuestions.length==0">
                <div class="row">
                  <div class="col-md-12">
                    No Record
                  </div>
                </div> 
              </div>

              <div class="accordion" id="accordionExample" v-else>
                    <div class="accordion-item" v-for="(item, index) in filteredQuestions" :key="item.id">

                      <h2 class="accordion-header" :id="'heading'+item.id">
                        <button class="accordion-button" :class="{ 'collapsed': index !== 0 }" type="button" data-bs-toggle="collapse" :data-bs-target="'#collapse'+item.id" aria-expanded="true" :aria-controls="'collapse'+item.id">
                          <strong>{{item.faqQuestion}}</strong>
                        </button>
                      </h2>
                      <div :id="'collapse'+item.id" class="accordion-collapse collapse"  :aria-labelledby="'heading'+item.id" data-bs-parent="#accordionExample">
                        <div class="accordion-body">
                          {{item.faqAnswer}}  <button type="button" class="btn btn-sm btn-outline-primary px-4 float-end" data-bs-toggle="tooltip" data-bs-placement="top" title="Delete Question" @click="deleteAccordionItem(item.id)"><i class="fas fa-trash-alt"></i></button> 
                          <button type="button" class="btn btn-sm btn-outline-primary px-4 float-end" @click="editFAQ(index,item)" data-bs-toggle="tooltip" data-bs-placement="top" title="Edit Question"><i class="fas fa-edit">
                          </i></button>
                          

                        </div>
                       
                      </div>

                    </div>
               </div>

            </div>

          </div>

          <!-- Modal -->
            <div class="modal fade" id="edit_faqModel" tabindex="-1" aria-labelledby="edit_faqModelLabel" aria-hidden="true" ref="myModal">
              <div class="modal-dialog modal-lg">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="edit_faqModelLabel">Edit FAQ</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                  </div>
                  <div class="modal-body">
                    <form class="was-validated" @submit.prevent="editData">
                        <div class="form-control mb-3">
                            <label for="validationQuestion" class="form-label"> Question</label>
                            <textarea class="form-control is-invalid"  id="validationQuestion" rows="3" v-model.trim="faqData.faqQuestion" placeholder="Required Question" required ref="descInput"></textarea>
                        </div>
                        <div class="form-control mb-3">
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

        </div>

      </div> 

  </div>
</template>

<script>
import addFaq from './addFaq.vue'
import { Modal } from 'bootstrap';
export default {

  name: 'faqList',
  components: {
    addFaq
  },
  data(){
    return{
      storeFAQ:JSON.parse(localStorage.getItem('FAQData')) || [],
      searchTerm:'',
      faqData:{},
      editModal:false,
      itemId:0,
      index:0,
    }
  },
  provide() {
    return {
      addFAQ: this.addFAQ,
      
    };
  },
  computed: {
    filteredQuestions() {
      return this.storeFAQ.filter(storeFAQ => {
        return storeFAQ.faqQuestion.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    }
  },
  
  methods:{
    addFAQ(enteredFAQ){
      this.storeFAQ.push(enteredFAQ);
      localStorage.setItem('FAQData', JSON.stringify(this.storeFAQ));

    },
    deleteAccordionItem(itemId) {
     this.storeFAQ = this.storeFAQ.filter(item => item.id !== itemId)
     localStorage.setItem('FAQData', JSON.stringify(this.storeFAQ));
  },
  editFAQ(index,item){
    this.editModal = new Modal(document.getElementById('edit_faqModel'), {})
    this.editModal.show()
    this.faqData.faqQuestion=item.faqQuestion;
    this.faqData.faqAnswer=item.faqAnswer;
    this.itemId=item.id;
    this.index=index
  },
  editData() {
    const index=this.index;
    const editedQuestion =this.faqData.faqQuestion;
    const editedAnswer =  this.faqData.faqAnswer;
    const editedId=this.itemId;
    this.storeFAQ.splice(index, 1, { faqQuestion: editedQuestion, faqAnswer: editedAnswer ,id:editedId});
    localStorage.setItem('FAQData', JSON.stringify(this.storeFAQ));
    this.editModal.hide();
  }
  }
}
</script>

<style scoped>



.form{

position: relative;
}

.form .fa-search{

position: absolute;
top:20px;
left: 20px;
color: #9ca3af;

}

.form span{
position: absolute;
right: 17px;
top: 13px;
padding: 2px;
border-left: 1px solid #d1d5db;

}

.left-pan{
padding-left: 7px;
}

.left-pan i{

padding-left: 10px;
}

.form-input{

height: 55px;
text-indent: 33px;
border-radius: 10px;
}




</style>