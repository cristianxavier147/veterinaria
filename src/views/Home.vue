<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-3 col-lg-2">
        <Aside></Aside>
      </div>
      <div class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
        <div class="row">
          <div class="col">
            <Header></Header>
          </div>
          <div class="col-12">
            <Content></Content>
          </div>
        </div>
      </div>
    </div>
    <p></p>
  </div>
</template>

<script>
import Aside from "@/components/Menu/Aside.vue";
import Header from "@/components/Menu/Header.vue";
import Content from "@/views/Content.vue";
export default {
  name: "Home",
  components: {
    Aside,
    Header,
    Content,
  },
};
</script>
