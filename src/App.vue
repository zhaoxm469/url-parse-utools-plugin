<template>
    <div class="container">
      <h1>URL参数解析器</h1>
      <div class="input-group" style="margin: 10px 0;">
        <label class="label">URL</label>
        <textarea
          v-model="url"
          class="textarea"
          placeholder="请输入或粘贴URL"
        ></textarea>
      </div>
  
      <div class="input-group" style="margin-bottom: 10px;">
        <label class="label">解析结果</label>
        <div class="json-editor">
          <textarea
            v-model="jsonString"
            class="textarea"
            @input="updateUrlFromJson"
            placeholder="URL参数将以JSON格式显示在这里"
            style="height: auto; min-height: 100px;"
          ></textarea>
        </div>
      </div>
  
      <button class="button" @click="copyToClipboard">复制参数到剪贴板</button>
    </div>
  </template>
  
  <script setup>
  import { ref, watch, onMounted } from 'vue';

  const url = ref('');
  const showEncoded = ref(false);
  const jsonString = ref('');
  const originalParams = ref({});

  const parseUrl = (url) => {
    try {
      const urlObj = new URL(url);
      const params = {};
      urlObj.searchParams.forEach((value, key) => {
        params[key] = showEncoded.value ? value : decodeURIComponent(value);
      });
      return params;
    } catch (e) {
      return {};
    }
  };

  const updateUrl = () => {
    try {
      const params = JSON.parse(jsonString.value);
      const urlObj = new URL(url.value);
      urlObj.search = '';
      
      for (const [key, value] of Object.entries(params)) {
        urlObj.searchParams.append(
          key,
          showEncoded.value ? encodeURIComponent(value) : value
        );
      }
      
      url.value = urlObj.toString();
    } catch (e) {
      console.error('Invalid JSON:', e);
    }
  };

  const updateUrlFromJson = (event) => {
    try {
      const params = JSON.parse(event.target.value);
      originalParams.value = params;
      updateUrl();
    } catch (e) {
      console.error('Invalid JSON:', e);
    }
  };

  const copyToClipboard = () => {
    try {
      const text = jsonString.value;
      utools.copyText(text);
      utools.showNotification('参数已复制到剪贴板');
      alert('参数已复制到剪贴板');
    } catch (e) {
      utools.showNotification('复制失败：' + e.message);
    }
  };

  onMounted(() => {
    // 监听 uTools 进入插件事件
    window.utools.onPluginEnter(({ code, type, payload }) => {
      if (type === 'regex' || type === 'over') {
        url.value = payload;
        originalParams.value = parseUrl(payload);
        jsonString.value = JSON.stringify(originalParams.value, null, 2);
      }
    });
  });

  watch(url, (newUrl) => {
    if (newUrl) {
      originalParams.value = parseUrl(newUrl);
      jsonString.value = JSON.stringify(originalParams.value, null, 2);
    }
  });
  </script> 