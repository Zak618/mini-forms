<script setup>
import { reactive, computed } from 'vue';
import axios from 'axios';

const review = reactive({
  author: '',
  stars: null,
  text: '',
  photo: null,
  isRecomended: true
})

const previewFilePath = computed(() => {
  if (review.photo) {
  return URL.createObjectURL(review.photo)
  }

  return '#'
})

const stars = [1, 2, 3, 4, 5]

const submit = () => {
  console.log('submit!');

  axios.post('/api/review', review, {headers: {
    'Content-Type': 'multipart/form-data' }
  })
  .then((res) => {
    console.log(res);
  })

  .catch((err) => {
    console.log(err);
  })
  
  .finally(() => {
    console.log('Конец!')
  })

}

const uploadFile = (e) => {
  const [file] = e.target.files;
  review.photo = file;
}

</script>

<template>
  <form class="container pt-5 pb-5" @submit.prevent.stop="submit">
    <input type="text" v-model="review.author" placeholder="Как вас зовут?" class="form-control">

    <textarea rows="3" class="form-control mt-3 mb-3" v-model="review.text" placeholder="Что понравилось?"></textarea>

    <h4>Оценка</h4>
    <div v-for="star in stars" :key="star" class="form-check">
      <input class="form-check-input" type="checkbox" :true-value="star" :false-value="null" :id="`star${star}`" v-model="review.stars">
      <label class="form-check-label" :for="`star${star}`">
        {{ star }}
      </label>
    </div>

    <div class="mt-3">
      <label for="formFile" class="form-label">Загрузите фото</label>
      <input class="form-control" type="file" @change="uploadFile">

      <img :src="previewFilePath" class="w-100 mt-2">
    </div>

    <div class="mt-3">
      <div class="form-check" mt-3>
        <input class="form-check-input" type="radio" name="flexRadioDefault" id="adv1" v-model="review.isRecomended" :value="false">
        <label class="form-check-label" for="adv1">
          Не советую
        </label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="radio" name="flexRadioDefault" id="adv2" v-model="review.isRecomended" :value="true">
        <label class="form-check-label" for="adv2">
          Советую!
        </label>
      </div>
    </div>

    <button class="btn btn-primary mt-4">Отправить</button>
  </form>
</template>
