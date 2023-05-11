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
              <div v-if="storeFAQ.length==0">
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
                          <strong>{{item.id+ ' . ' +item.faqQuestion}}</strong>
                        </button>
                      </h2>
                      <div :id="'collapse'+item.id" class="accordion-collapse collapse"  :aria-labelledby="'heading'+item.id" data-bs-parent="#accordionExample">
                        <div class="accordion-body">
                          {{item.faqAnswer}}  <button type="button" class="btn btn-sm btn-outline-primary px-4 float-end" data-bs-toggle="tooltip" data-bs-placement="top" title="Delete Question" @click="deleteAccordionItem(item.id)"><i class="fas fa-trash-alt"></i></button> 
                          <button type="button" class="btn btn-sm btn-outline-primary px-4 float-end" data-bs-toggle="tooltip" data-bs-placement="top" title="Edit Question"><i class="fas fa-edit">
                          </i></button>
                          

                        </div>
                       
                      </div>

                    </div>
               </div>

            </div>

          </div>

        </div>

      </div> 

  </div>
</template>

<script>
import addFaq from './addFaq.vue'
export default {

  name: 'faqList',
  components: {
    addFaq
  },
  data(){
    return{
      storeFAQ:JSON.parse(localStorage.getItem('FAQData')) || [],
      searchTerm:'',
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