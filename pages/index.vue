<template>
  <div>
    <div class="my-3">
      <div class="mb-4">
        <form @submit.prevent="getData" class="d-flex" role="search">
          <input
          v-model = "keyword"
          class="form-control"
          type="search"
          placeholder="Search"
          aria-label="Search">
        </form>
      </div>
    </div>

<div class="row">
  <div v-for="book in books" :key="book.id" class="col-2">
    <div class="card">
      <NuxtLink :to="`/detail/${book.id}`">
        <div class="card-header">
        <img :src="book.cover" alt="Cover" class="Cover">
      </div>
      </NuxtLink>
    </div>
  </div>
</div>

    
  </div>
</template>


<script setup>
const supabase = useSupabaseClient()
const books = ref([])
const keyword = ref([])

onMounted(() => getData())

async function getData() {
  let { data, error } = await supabase
  .from('buku')
  .select(`
  id, judul, penulis, penerbit, cover,
  kategori(nama), rak(kode)
  `)
    .ilike('judul', `%${keyword.value}%`)
  if(data) books.value = data
  if (error) throw error
}
</script>

<style scoped>
.Cover{
  width: 100%;
}
</style>

