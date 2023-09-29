<script>
 import { onMount } from 'svelte';
    let operator = '';
    let result = '';

    let input1, input2;
    let currentCurrency = "vnd"; // 기본 선택값 설정
    let dstCurrency = "krw"; // 기본 선택값 설정
    let srcMoney = "0";
    let dstMoney = "0";
    let exchangeData = null;

    let tid = null;

    onMount(()=>getCurrency());
    function getCurrency() {
            // 웹 서비스 요청 (기본 JavaScript)
        fetch(`https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/${currentCurrency}/${dstCurrency}.min.json`)
        .then(response => response.json())
        .then(data => {exchangeData = data; delayCalc();})
        .catch(error => console.error('Error:', error));
    }
    function numberWithCommas(x) {
    // 소수점 이하 자릿수와 천 단위 구분 컴마 추가
    return x.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  }
    function handleClickCalc() {
        if (exchangeData) {
            dstMoney = numberWithCommas(parseFloat(srcMoney) * exchangeData[dstCurrency]);
        }
    }
    function delayCalc() {
        if (tid) {
            clearTimeout(tid);
        }
        tid = setTimeout(function() {
        // 1초 후 실행할 코드를 여기에 작성합니다.
            if (srcMoney === '')
                srcMoney ='0';
            if (!srcMoney.endsWith('.'))
                srcMoney = parseFloat(srcMoney) + '';
            handleClickCalc();
        }, 1000);
    }
//$: srcMoney = Number(srcMoney) + '';
    function handleClick(c) {
        if (c !== '' && srcMoney === '0')
            srcMoney = c;
        else
            srcMoney += c;
        delayCalc();
    }
    function handleClickDelete() {
        srcMoney = srcMoney.slice(0,-1)
        delayCalc();
    }
  function handleSelectChange(event) {
    //currentCurrency = event.detail.value;
    console.log('cur' + currentCurrency);
    console.log('dst' + dstCurrency);
    getCurrency();
    
  }
  function handleClickClear() {
      srcMoney = "0";
      delayCalc();
  }
    
</script>
<div class="bg-white p-4 rounded-lg shadow-md w-80">
  <div class="mb-4 flex items-center">
        <select
        class="block w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
        bind:value={currentCurrency}
        on:change={handleSelectChange}
        >
        <option value="vnd">베트남 동</option>
        <option value="usd">미국 달러</option>
        <option value="krw">대한민국 원</option>
        </select>
        <input bind:value={srcMoney} type="text" on:input={delayCalc} class="w-3/4 ml-2 border border-gray-300 p-2 rounded" placeholder="숫자를 입력하세요">
    </div>
    <div class="mb-4 flex items-center">
        <select
        class="block w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
        bind:value={dstCurrency}
        on:change={handleSelectChange}
        >
        <option value="vnd">베트남 동</option>
        <option value="usd">미국 달러</option>
        <option value="krw">대한민국 원</option>
        </select>
        <input bind:value={dstMoney} type="text" class="w-3/4 ml-2 border border-gray-300 p-2 rounded" placeholder="숫자를 입력하세요" readonly>
    </div>
    <div class="grid grid-cols-4 gap-2 p-4">
    <button class="btn btn-primary col-span-2" on:click={delayCalc}>Calc</button>
    <button class="btn btn-primary col-span-2" ></button>
 
    <button class="btn btn-primary" on:click={() => handleClick('7')}>7</button>
    <button class="btn btn-primary" on:click={() => handleClick('8')}>8</button>
    <button class="btn btn-primary" on:click={() => handleClick('9')}>9</button>
    <button class="btn btn-primary" on:click={() => handleClickDelete()}>&lt;</button>
    
    <button class="btn btn-primary" on:click={() => handleClick('4')}>4</button>
    <button class="btn btn-primary" on:click={() => handleClick('5')}>5</button>
    <button class="btn btn-primary" on:click={() => handleClick('6')}>6</button>
    <button class="btn btn-primary" on:click={() => handleClick('')}></button>
    <button class="btn btn-primary" on:click={() => handleClick('1')}>1</button>
    <button class="btn btn-primary" on:click={() => handleClick('2')}>2</button>
    <button class="btn btn-primary" on:click={() => handleClick('3')}>3</button>
    <button class="btn btn-primary" on:click={() => handleClick('.')}>.</button>
    
    <button class="btn btn-primary" on:click={() => handleClick('0')}>0</button>
    <button class="btn btn-primary" on:click={() => handleClick('00')}>00</button>
    <button class="btn btn-primary" on:click={() => handleClick('000')}>000</button>
    
    <button class="btn btn-error" on:click={() => handleClickClear()}>C</button>
    </div>
</div>