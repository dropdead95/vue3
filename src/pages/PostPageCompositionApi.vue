<template>
  <div>
    <h1>Страница с постами</h1>
        <my-input v-focus v-model="searchQuery" placeholder="Поиск..."></my-input>
        <div class="app__btns">
          <my-button> Создать пост </my-button>
          <my-select v-model="selectedSort" :options="sortOptions"></my-select>
        </div>

        <my-dialog v-model:show="dialogVisible">
          <post-form />
        </my-dialog>
        <post-list
            v-if="!isPostsLoading"
            :posts="sortedAndSearchedPosts"
        ></post-list>
        <div v-else>Идет загрузка...</div>

  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyDialog from "@/components/UI/MyDialog";
import MyButton from "@/components/UI/MyButton";
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";
import { ref } from "vue";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedandSearchedPosts";
import {usePosts} from "@/hooks/usePosts";

export default {
  components: {
    MyDialog,
    PostList,
    PostForm,
    MyButton,
    MySelect,
    MyInput,
  },
  data() {
    return {
      dialogVisible: false,
      sortOptions: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По описанию " },
      ],
    };
  },
  setup(props) {
    const { posts, totalPages, isPostsLoading } = usePosts(10);
    const { selectedSort, sortedPosts } = useSortedPosts(posts);
    const { searchQuery, sortedAndSearchedPosts } =
      useSortedAndSearchedPosts(sortedPosts);

    return {
      posts,
      selectedSort,
      totalPages,
      isPostsLoading,
      searchQuery,
      sortedAndSearchedPosts,
    };
  },
};
</script>

<style>
.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
.page__wrapper {
  display: flex;
  align-items: center;
  margin-top: 15px;
}
.page {
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
  margin-right: 10px;
}
.current-page {
  border: 3px solid teal;
}
.observer {
  height: 30px;
  background: green;
}
</style>
