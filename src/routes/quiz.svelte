<script>
  import { onMount } from 'svelte';
  let item;
  let itemKeys;
  let itemValues;
  let allQuestion = [];
  let score;
  let choice = ["A", "B", "C", "D"];
  let getResult = false;
  const alertUser = (e) => {
    e.preventDefault();
    e.returnValue = "";
  };
  async function getData() {
    item = await fetch('https://raw.githubusercontent.com/FWcloud916/tw-boating-exam-convert/main/output/1110210_exam.json').then(r => r.json());
    itemKeys = {
      "weather-common": 3,
      "law-crime": 3,
      "law-sea": 1,
      "law-cert": 3,
      "law-boat": 3,
      "sail-common-way": 3,
      "sail-common-mark": 3,
      "sail-common-engine": 4,
      "sail-common-operate": 4,
      "sail-common-maintain": 3,
      "seamanship-basic": 3,
      "seamanship-equip": 3,
      "seamanship-tech": 3,
      "communication-management": 3,
      "communication-emergency": 2,
      "sail-way": 3,
      "sail-light": 3
    };
    function getRandomArrayElements(arr, count) {
      var shuffled = arr.slice(0), i = arr.length, min = i - count, temp, index;
      while (i-- > min) {
          index = Math.floor((i + 1) * Math.random());
          temp = shuffled[index];
          shuffled[index] = shuffled[i];
          shuffled[i] = temp;
      }
      return shuffled.slice(min);
    }

    Object.keys(itemKeys).forEach(element => {
      const temp = getRandomArrayElements(Object.values(item[element]), itemKeys[element]);
      temp.forEach(q => {
        allQuestion.push(q)
      })
    });
    console.log(allQuestion)
    allQuestion = allQuestion;

    window.addEventListener("beforeunload", alertUser);
  }

  onMount(getData);
  const submit = () => {
    getResult = true;
    console.log(allQuestion);
    const filterCorrect = allQuestion.filter(q => q.answer === q.userAnswer);
    score = filterCorrect.length * 2;
    window.scrollTo({
      top: 100,
      left: 100,
      behavior: 'smooth'
    });
  }
  const handleChange = (e) => {
		const index = e.target.dataset.index;
    const value = e.target.value;
    allQuestion[index].userAnswer = value;
	};
 
</script>
<div>
  {#if allQuestion.length > 0}
  {#if getResult}
  <div class="alert alert-secondary" role="alert">
    您的分數為 {score} 分。
    {#if score > 75} <b> 通過 </b> {:else} <b> 沒有通過 </b>{/if}
    學科測驗標準。
  </div>
  <hr />
  {/if}
  {#each allQuestion as qus, index}
  <div class="card mb-2">
    <div class="card-body">
      {#if getResult}
        {#if qus.answer === qus.userAnswer}
        <div class="alert alert-success" role="alert">
          答案正確。
        </div>
        {:else}
        <div class="alert alert-danger" role="alert">
          答案錯誤，正確答案為 {qus.answer}
        </div>
        {/if}
      {/if}
      <p class="card-title mb-0">{index + 1} . {qus.question}</p>
      {#if qus.img}
        <img src="https://raw.githubusercontent.com/FWcloud916/tw-boating-exam-convert/main/{qus.img}" class="img-fluid" />
      {/if}
    </div>
    <ul class="list-group list-group-flush">
      {#each choice as c}
      <li class="list-group-item">
        <div class="form-check">
          <input class="form-check-input" data-index="{index}" type="radio" name={qus.question} id={`${qus.question}_${c}`} on:change={handleChange} value="{c}" disabled="{getResult}">
          <label class="form-check-label w-100" for={`${qus.question}_${c}`}>
            {qus[c]}
          </label>
        </div>
      </li>
      {/each}
    </ul>
  </div>
	{/each}
  <button type="button" class="btn btn-primary" on:click={submit}>送出答案</button>
  {:else}
      <p>loading.....</p>
  {/if}
</div>