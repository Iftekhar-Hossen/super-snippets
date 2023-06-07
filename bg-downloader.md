### Background File Download 

```javascript
function downloadFile(url, saveAs) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.responseType = "blob";

    xhr.onload = () => {
      if (xhr.status === 200) {
        const blob = xhr.response;
        const reader = new FileReader();

        reader.onloadend = () => {
          const fileData = reader.result;
          const a = document.createElement("a");
          a.href = fileData;
          a.download = saveAs;
          document.body.appendChild(a);
          a.click();
          resolve();
        };

        reader.readAsDataURL(blob);
      } else {
        reject(new Error(`Download failed. Status: ${xhr.status}`));
      }
    };

    xhr.onerror = () => {
      reject(new Error("Download failed. Network error."));
    };

    xhr.onprogress = (event) => {
      if (event.lengthComputable) {
        const percentComplete = (event.loaded / event.total) * 100;
        console.log(`Download progress: ${percentComplete.toFixed(2)}%`);
      }
    };

    xhr.send();
  });
}

// Example usage
const fileUrl = "https://i.dummyjson.com/data/products/11/2.jpg";
const fileName = "downloaded_file.jpg";

downloadFile(fileUrl, fileName)
  .then(() => {
    console.log("Download completed and saved.");
  })
  .catch((error) => {
    console.error(error);
  });

```
