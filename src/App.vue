<template>
  <div class="container">
    <div class="options"></div>
    <div id="discription"></div>
  </div>
</template>

<style>
.container {
  background-color: #ffffff;
  width: 90vmin;
  padding: 50px 30px;
  position: absolute;
  transform: translate(-50%, -50%);
  left: 50%;
  top: 50%;
  border-radius: 10px;
  box-shadow: 0 25px 50px rgba(7, 20, 35, 0.2);
}
.options {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}
#discription {
  margin-top: 10px;
  border: none;
  padding: 20px;
  height: 50vh;
  overflow-y: auto;
}
button {
  /*margin-top: 5px;*/
  border: none;
  border-radius: 5px;
  color: #ffffff;
  background-color: rgb(244, 53, 145);
  width: 100%;
  height: 4vh;
  cursor: pointer;
}
</style>

<script>
import $ from "jquery";
//import jQuery from "jquery";
import "select2";
import "select2/dist/css/select2.css";

export default {
  name: "App",
  data() {
    return {};
  },
  mounted() {
    let baseUrl = window.location.origin;
    let jsonUrl = baseUrl + "/data/GRE3000.json";
    let selectEle = document.createElement("select");
    let optionsEle = document.querySelector(".options");
    let buttonEle = document.createElement("button");
    let disEle = document.getElementById("discription");
    let pEle = document.createElement("p");
    selectEle.id = "select";
    selectEle.style.width = "100%";
    optionsEle.appendChild(selectEle);
    $(() => {
      $("#select").select2();
    });

    buttonEle.innerHTML = "SEARCH";
    optionsEle.appendChild(buttonEle);

    disEle.appendChild(pEle);

    fetch(jsonUrl)
      .then((response) => response.json())
      .then((data) => {
        let words = Object.keys(data);
        console.log(words);
        words.sort();
        for (let i = 0; i < words.length; ++i) {
          let tempEle = document.createElement("option");
          tempEle.innerHTML = words[i];
          selectEle.appendChild(tempEle);
        }

        buttonEle.addEventListener("click", () => {
          let word = selectEle.options[selectEle.selectedIndex].text;
          let textList = data[word];
          disEle.removeChild(pEle);
          pEle = document.createElement("p");
          disEle.appendChild(pEle);
          for (let i = 0; i < textList.length; ++i) {
            let tempEle = document.createElement("span");
            tempEle.innerHTML = textList[i];
            pEle.appendChild(tempEle);
            tempEle = document.createElement("br");
            pEle.appendChild(tempEle);
          }
        });
      });
  },
};
</script>
