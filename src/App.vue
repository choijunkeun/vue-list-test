<template>
  <div>
    <h1>배열 API 테스트</h1>
    <div>
      <input type="text" v-model="query"
      placeholder="Enter text"
      /> {{ query }}
      <button @click="onClickForEach">forEach</button>
      <button @click="onClickFilter">filter</button>
      <button @click="onClickMap">map</button>
      <button @click="onClickReduce">reduce</button>
      <button @click="onClickPush">push</button>
      <button @click="onClickPop">pop</button>
      <button @click="onClickSlice">slice</button>
      <button @click="onClickSplice"> splice</button>
      <button @click="onClickIndexOf">indexOf</button>
      <button @click="onClickIncludes">includes</button>
      <button @click="onClickFind">find</button>
      <button @click="onClickSome">some</button>
      <button @click="onClickEvery">every</button>
      <button @click="onClickSort">sort</button>
      <button @click="onClickJoin">join</button>
    </div>
    <div>
      <strong>Array</strong> : {{array.join(", ")}}

    </div>
    <div>
      <strong>Result</strong> : {{result}}
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return{
      array : ["cherry", "apple", "grape", "banana", "kiwi"],
      query : "",
      result: "",

    }
  },

  methods: {
    // 아래의 모든 함수는 배열에 대한 함수
    onClickForEach() {
      let result = "";
      this.array.forEach((item, index) => {
        result += `${index}: ${item}, `;
      });
      this.result = result.slice(0, -2);
    },
    onClickFilter() { 
        // includes(param) -->> param으로 들어온놈 있냐? 있으면 true, 없으면 false
        // filter()        -->> 배열 내부 돌면서 true인놈들만 걸러서 새로운 배열로 리턴)
      const onClickFilter = this.array.filter((item) => item.includes(this.query));
      this.result = onClickFilter.join(", ");
    },
    onClickMap() { 
        // map()           -->> 배열 내부 돌면서 새로운 배열으로 return, 안에 특정 로직을 넣어 사용
      const onClickMap = this.array.map((item) => item.toUpperCase());
      this.result = onClickMap.join(", ");
    },
    onClickReduce() {
        // reduce()        -->> 배열의 각 요소에 대해 주어진 리듀서(reducer)함수를 실행하고, 하나의 결과를 반환
        // acc는 누산기, cur은 현재값
        // 첫번쨰 loop : acc = cherry                                     , cur = apple
        // 두번쨰 loop : acc = cherry + apple                             , cur = grape
        // 세번쨰 loop : acc = cherry + apple + grape                     , cur = banana
        // 네번쨰 loop : acc = cherry + apple + grape + banana            , cur = kiwi
        // 최종 리턴    : acc = cherry + apple + grape + banana + kiwi    
      const onClickReduce = this.array.reduce((acc, cur) => {
        console.log("자 루프돌자");
        console.log(`acc ::: ${acc} ,,,,    cur ::: ${cur}`);
        return `${acc} + ${cur}`
      });
      this.result = onClickReduce;
    },
    onClickPush() {
        // 배열 마지막에 값 넣기
      const newArray = [...this.array, this.query];
      this.result = newArray.join(", ");
    },
    onClickPop() {
       // 배열 뒤부터 값 빼기
      const newArr = [...this.array];
      newArr.pop();
      this.array = newArr;
      this.result = newArr.join(", ");
    }, 
    onClickSlice() {
        // 앞의 idx 부터(포함) 뒤의 idx(미포함) 전까지 잘라내서 새로운 배열로 반환 (ex: slice(1,4) >> 1 <= x < 4)
      const onClickSlice = this.array.slice(1, 4);
      this.result = onClickSlice.join(", ");
    },
    onClickSplice() {
      // (시작idx, 시작 idx부터 몇개 잘라낼건지, 새로 넣을 값들 주르륵)
      // 아래 코드는 2번째 인덱스 부터 2개의 인덱스를 잘라내고 뒤에 들어온 값들을 채워넣어 새로운 배열로 반환
      const newArr = [...this.array];
      newArr.splice(2, 2, "간장", "치킨", "이렇게!", "이렇게?", "장충동", "왕족발");
      this.array = newArr;
      this.result = newArr.join(", ");
    },
    onClickIndexOf() {
      // 배열의 몇번째 인덱스에 있냐?
      const onClickIndexOf = this.array.indexOf(this.query);
      this.result = onClickIndexOf.toString();
    },
    onClickIncludes() {
      // 파라미터로 들어온놈 배열안에 있냐? 있으면 true, 없으면 false
      const includes = this.array.includes(this.query);
      this.result = includes.toString();
    },
    onClickFind() {
      // 배열 돌면서 주어진 함수 만족하는 첫번째 놈 반환
      // includes는 파라미터로 들어온놈 있냐? 니까 처음 true(있냐? 에 걸린놈)로 뜬 놈 반환
      const onClickFind = this.array.find((item) => item.includes(this.query));
      this.result = onClickFind || "Not Found";
    },
    onClickSome() {
      // 배열 돌면서 주어진 함수 하나라도 만족하면 true 반환, 없으면 false반환
      const onClickSome = this.array.some((item) => item.includes(this.query));
      this.result = onClickSome.toString();
    },
    onClickEvery() {
      // 배열 돌면서 주어진 함수(조건)이 배열안의 모든놈들에게 만족하는지, 만족하면 true 한놈이라도 만족못하면 false
      const onClickEvery = this.array.every((item) => item.length > 4);
      this.result = onClickEvery.toString();
    },
    onClickSort() {
      // 기본정렬(유니코드순)
      // 숫자크기나 그런걸로 하려면 파라미터 함수로 정의해줘야함
      // ex) [1,3,2,4].sort(function(a, b) {  // 오름차순
      //        return a - b;
      //      })
      const onClickSort = [...this.array].sort()
      this.array = onClickSort;
      this.result = onClickSort.join(", ");
    },
    onClickJoin() {
      // 배열 사이사이에 원하는 값 끼워넣는거임
      const onClickJoin = this.array.join("🎸 ")
      this.result = onClickJoin;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
