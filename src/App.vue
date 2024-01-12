<script setup>
import {ref} from 'vue'
import { useToast } from "vue-toastification";
const toast = useToast();


const showModal = ref(false)
function openModal(action) {
  showModal.value = action;
}
const newNote = ref('');
const randombgcolor = () => {
  return "hsl(" + Math.random() * 360 + ",100%,75%)"
}


const notes = ref([])

function createNote() {
if (newNote.value.trim().length > 9) {
    const randomId = Math.floor(Math.random() * 1000000);
    const note = {
      noteText: newNote.value.trim(),
      noteid: randomId,
      bgColor: randombgcolor(),
      date: new Date().toLocaleDateString()
    }
    notes.value.push(note)
    openModal(false)
    newNote.value = ''
    toast.success('Note Added Successfully')

    return true
}else{
toast.error('Note mut be 10 character minimun')
}
}

</script>


  <template>
    <header class="p-5 w-full flex justify-between"> 
      <h1 class="text-xl font-bold">NOTE</h1>
      <button @click="openModal(true)" class=" h-9 w-9 bg-black text-white font-bold rounded-full">+</button>
    </header>

    
    <div v-if="showModal" class="absolute bg-black  h-full w-full bg-opacity-75 flex justify-center items-center ">
      <form class="flex flex-col justify-center items-center w-8/12 p-10 bg-white rounded-xl">
        <textarea v-model="newNote" cols="30" rows="5"
          class="p-1 bg-transparent border-2 border-black text-black outline-none w-full rounded-md"
          placeholder="Enter new note text here"></textarea>
        <button @click.prevent="createNote" class="bg-purple-800 p-3 w-full text-white font-bold mt-5 rounded-md">Add
          Note</button>
        <button @click="openModal(false)" type="button"
          class="bg-red-800 p-3 w-full text-white font-bold mt-5 rounded-md">Close</button>
      </form>
    </div>
    <main class="p-5 grid grid-cols-2 gap-6">

      <div v-for="note in notes" :key="note.noteid" :style="{backgroundColor: note.bgColor }" class="p-3 rounded-xl w-9/12  flex flex-col justify-between">
        <P class="text-black break-words mb-10">{{  note.noteText }}
        </P>
        <small class="font-bold">{{note.date }}</small>
      </div>

    </main>
</template>




