# dowload-youtube-video-list.

Скрипт загрузки массива youtube видео через сервис [`https://x2mate.com/ru16`](https://x2mate.com/ru16).


```javascript
let data = ["https://www.youtube.com/watch?v=Xa_vTdYH-sw","https://www.youtube.com/watch?v=hxeOnWfHWVc","https://www.youtube.com/watch?v=yq3qpxDnyKY"];

async function worker123(data) {
  let searchInput = document.getElementById('s_input');
  let searchBtn = document.querySelector('[onclick="ksearchvideo()"]');
  for (let i = 0; i < data.length; i++) {
    let href = data[i];
    let index = i + 1;
    console.log(`${index}/${data.length}) ${href}: Загрузка начинается.`);
    searchInput.value = href;
    await new Promise((resolve) => setTimeout(resolve, 500));
    searchBtn.click();
    await new Promise((resolve) => setTimeout(resolve, 2000));
    await new Promise((resolve) => {
      let counter = 0
      let interval = setInterval(function () {
        if (document.getElementById('loader-wrapper')?.style?.display === 'none') {
          clearInterval(interval);
          resolve()
        } else {
          counter++;
          console.log(`${index}/${data.length}) ${href}: (${counter}) Ожидание лоадера выбора качества...`);
        }
      }, 1000);
    })
    let formatSelect = null;
    await new Promise(function (resolve) {
      let counter = 0
      let interval = setInterval(function () {
        formatSelect = document.getElementById('formatSelect');
        if (formatSelect) {
          clearInterval(interval);
          resolve();
        } else {
          counter++;
          console.log(`${index}/${data.length}) ${href}: (${counter}) Не найден выбор качества.`);
        }
      }, 1000);
    });
    let value = formatSelect?.querySelectorAll('[label="mp4"] option')
    if (!value) {
      console.log(`${index}/${data.length}) ${href}: Пропущен, не найден выбор качества mp4.`);
      continue;
    }
    formatSelect.value = value[0]?.value;
    let event = new Event('change');
    formatSelect.dispatchEvent(event);
    await new Promise((resolve) => setTimeout(resolve, 1000));
    let getLinkBtn = document.querySelector('[onclick="convertFile(0)"]');
    if (!getLinkBtn) {
      console.log(`${index}/${data.length}) ${href}: Пропущен, не найдена кнопка генерации ссылки.`);
      continue;
    }
    getLinkBtn.click()
    await new Promise((resolve) => setTimeout(resolve, 1000));

    let downloadBtn = document.getElementById('asuccess');
    await new Promise(function (resolve) {
      let counter = 0
      let interval = setInterval(function () {
        if (downloadBtn.href !== '#' && document.getElementById('mesg-convert')?.classList?.contains('hidden')) {
          clearInterval(interval);
          resolve();
        } else {
          counter++;
          console.log(`${index}/${data.length}) ${href}: (${counter}) Ожидание преобразования ссылки...`);
        }
      }, 1000);
    });
    await new Promise((resolve) => setTimeout(resolve, 1000));
    downloadBtn.click();
    console.log(downloadBtn.href)
    await new Promise((resolve) => setTimeout(resolve, 20000));
    downloadBtn.href = '#'
    console.log(`${index}/${data.length}) ${href}: Загрузка завершена.`);
  }
  console.log('Загрузка завершена.')
}

worker123(data);
```
