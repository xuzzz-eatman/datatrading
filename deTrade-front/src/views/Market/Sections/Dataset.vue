<script setup>
import { ref, computed } from "vue";
import ExampleCard from "../../../components/ExampleCard.vue";

const props = defineProps({
  data: {
    type: Array,
    required: true,
    default: () => [],
  },
  col1: {
    type: String,
    default: "col-lg-3",
  },
  col2: {
    type: String,
    default: "col-lg-9",
  },
});

// 搜索相关的响应式数据
const searchQuery = ref("");
const filteredItems = ref(props.data.map(item => item.items).flat()); // 初始化为所有数据集

// 搜索方法
const searchItems = () => {
  if (!searchQuery.value) {
    filteredItems.value = props.data.map(item => item.items).flat(); // 如果没有搜索，返回所有项目
    return;
  }

  const query = searchQuery.value.toLowerCase();
  filteredItems.value = props.data.map(item => item.items).flat().filter(item => {
    return (
      item.title.toLowerCase().includes(query) ||  
      item.subtitle.toLowerCase().includes(query)
    );
  });
};
</script>

<script>
export default {
  inheritAttrs: false,
};
</script>

<template>
  <section class="my-5 py-5">
    <div class="container">
      <div class="row justify-content-center text-center my-sm-5">
        <div class="col-lg-6">
          <h2 class="text-dark mb-0">探索丰富的数据集</h2>
          <p class="lead">
            我们提供多样化的数据集供您选择，助力您的研究与决策。
          </p>
          <!-- 搜索框 -->
          <div class="input-group mb-3">
            <input
              v-model="searchQuery"
              type="text"
              class="form-control border-secondary"  
              placeholder="搜索数据集..."
            />
            <button
              @click="searchItems"  
              class="btn btn-success"  
            >
              搜索
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="container mt-sm-5 mt-3">
      <div
        v-for="({ heading, description, items }, index) in data"
        :class="`row ${index !== 0 ? 'pt-lg-6' : ''}`"
        :key="heading"
      >
        <div :class="`${col1}`">
          <div
            class="position-sticky pb-lg-5 pb-3 mt-lg-0 mt-5 ps-2"
            style="top: 100px"
          >
            <h3>{{ heading }}</h3>
            <h6 class="text-secondary font-weight-normal pe-3">
              {{ description }}
            </h6>
          </div>
        </div>
        <div :class="`${col2}`">
          <div :class="`row ${index !== 0 ? 'mt-3' : ''}`">
            <div
              class="col-md-4 mt-md-0"
              v-for="{ image, title, subtitle, route, id, pro } in filteredItems"
              :key="title"
            >
              <ExampleCard
                class="min-height-160 shadow-lg mt-4"
                :image="image"
                :title="title"
                :subtitle="subtitle"
                :route="route"
                :id = "id"
                :pro="pro"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
