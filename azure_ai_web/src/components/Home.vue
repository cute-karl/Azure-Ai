<template>
  <div class="con">
    <div>
      <h2>文本审核</h2>
      <textarea v-model="text"></textarea>
      <button @click="uploadText">上传文本</button>
    </div>
    <div>
      <h2>图片审核</h2>
      <input type="file" @change="onFileChange" accept="image/*" />
      <button @click="uploadImage">上传图片</button>
    </div>
    <div>
      <h2>短视频审核</h2>
      <input type="file" @change="onFileChange" accept="video/*" />
      <button @click="uploadVideo">上传短视频</button>
    </div>
    <div v-if="response">
      <h2>审核结果</h2>
      <pre>{{ response }}</pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      text: '',
      file: null,
      response: null,
    };
  },
  methods: {
    onFileChange(e) {
      this.file = e.target.files[0];
    },
    async uploadText() {
      try {
        // 确保在上传文本时，传递正确的参数格式
        const response = await axios.post('/api/text', null, {
          params: { text: this.text }
        });
        console.log("文本审核已发送")
        console.log(response.data);
        this.response = response.data.Result;
      } catch (error) {
        console.error('Error uploading text:', error);
        this.response = 'Error uploading text';
      }
    },
    async uploadImage() {
      if (!this.file) {
        alert('Please select an image file.');
        return;
      }
      const formData = new FormData();
      formData.append('file', this.file);
      try {
        const response = await axios.post('/api/image', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        console.log("图片审核已发送")
        this.response = response.data.conclusion;
      } catch (error) {
        console.error('Error uploading image:', error);
        this.response = 'Error uploading image';
      }
    },
    async uploadVideo() {
      if (!this.file) {
        alert('Please select a video file.');
        return;
      }
      const formData = new FormData();
      formData.append('file', this.file);
      try {
        const response = await axios.post('/api/video', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        console.log("视频审核已发送")
        this.response = response.data.conclusion;
      } catch (error) {
        console.error('Error uploading video:', error);
        this.response = 'Error uploading video';
      }
    }
  }
};
</script>

<style>
.con {
  position: absolute;
  left: 50%;
  top:50%;
  transform: translate(-50%,-50%);
}

h1, h2 {
  color: #2c3e50;
}

textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
}

button {
  margin-top: 10px;
}

pre {
  background: #f0f0f0;
  padding: 10px;
}
</style>
