<template>
  <div>
    <!-- h1 class="title">Web Publisher URL builder</h1 -->
    <div class="form">
  
      <div  class="field">
        <label for="url" class="label">Repo URL on GitHub</label>
        <input id="url" type="text" v-model="url" class="input"
          placeholder="github.com/account/repository" required />
      </div>
      <div class="field">
        <label for="branch" class="label">
           Branch
        </label>
        <input id="branch" type="text" v-model="branch" 
        required class="input" 
        placeholder="e.g. main"/>
      </div>
      <div  class="field" style="grid-column: 1/-1">
      <label for="path" class="label" >
        File Path
        </label>
        <input id="path" type="text" v-model="path" class="input" 
          placeholder="File path to the specific file in the repo to be edited"
          required />
      </div>
      <div class="field" style="grid-column: 1/-1">
      <label for="initialPath" class="label">
        Initial Page, <span class="optional">optional</span>
        </label>
        <input id="initialPath" type="text" v-model="initialPath" class="input" placeholder="URL path to be rendered in preview" />
      </div>
      <fieldset class="radio-group" style="grid-column: 1/-1">
        <legend class="label">View</legend>
        <div class="radio-group-btns">
        <label class="radio-label">
          <input type="radio" name="view" v-model="view" value="preview" class="radio"> 
          <span>
            Preview
            </span>
        </label> 
        <label class="radio-label">
          <input type="radio" name="view" v-model="view" value="editor" class="radio"> 
          <span>
          Editor
          </span>
        </label>
        <label class="radio-label">
        <input type="radio" name="view" v-model="view" :value="null" class="radio"> 
        <span>
        Preview + Editor
        </span>
        </label>
      </div>
      </fieldset>
    </div>
    <div class="result-wrapper">
      <div class="result-title">Result</div>
      <div class="result-text-wrapper">
      <span class="result-placeholder" v-show="!result">You’ll see the result of your actions here</span>
      <span class="result" v-show="result">
      {{ result }}</span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return { url: '', branch: '', path: '', initialPath: '', view: null };
  },
  methods: {
    copy(event) {
      navigator.clipboard
        .writeText(this.result)
        .then(() => {
          console.info('copied');
        })
        .catch((e) => {
          console.info('Failed to copy', e);
        });
    },
  },
  computed: {
    result: (vm) => {
      // https://github.com/stackblitz/stackblitz
      const data = /(?:https:\/\/)?github\.com\/([^/]+)\/([^/]+)/.exec(vm.url);
      
      if (!data || data.length < 2 || !vm.branch | !vm.path) {
        return '';
      }
      const owner = data[1];
      const repo = data[2];
      const url = new URL(`${owner}/${repo}/edit/${vm.branch}/${vm.path}`, "https://pr.new/")

      if (vm.initialPath) {
        url.searchParams.append("initialPath", vm.initialPath)
      }
    
      if (vm.view) {
        url.searchParams.append("view", vm.view)
      }

      return url.toString()
    },
  },
};
</script>

<style>

:root {
  --sb-brand-blue: #1389FD;
}

* {
  box-sizing: border-box;
}

body {
 margin: 0;
 padding: 0;
 width: 100%;
 box-sizing: border-box;
}

#app {
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 12px 12px 6px;
}

.title {
  margin-block-end: 24px;
  text-align: left;

  font-weight: 700;
  font-size: 32px;
  line-height: 39px;
  letter-spacing: -0.015em;
  color: #000000;
}

.form {
  display: grid;
  grid-template-columns: minmax(100px, 1fr) minmax(100px, 1fr);
  grid-column-gap: 30px;
}


.field {
  margin-block-end: 16px;

  display: flex;
  flex-flow: column;
  align-items: flex-start;
}

.label {
  margin-block-end: 8px;

  font-weight: 500;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: -0.015em;
  color: #000000;
}

.input {
  padding: 10px 8px;

  width: 100%;
  border: 1px solid rgba(0, 0, 0, 0.15);
}

.input:focus {
  outline: 2px solid var(--sb-brand-blue);
}

.optional {
  font-style: italic;
  font-weight: 500;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: -0.015em;
}

.radio-label {
  display: block;
  padding: 10px 0;
  position: relative;
}

.radio-group {
  border: none;
  text-align: left;
  padding: 0;
  margin: 0;
}

.radio {
  position: absolute;
  appearance: none;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.radio ~ span {
  padding: 10px 8px;

  font-weight: 500;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: -0.015em;
  border: 1px solid rgba(0, 0, 0, 0.15);
  color: var(--sb-brand-blue);
}

.radio-label:not(:first-child) > .radio ~ span {
  border-left: none;
}

.radio:focus ~ span {
  outline: 2px solid var(--sb-brand-blue);
}

.radio:checked ~ span {
  background-color: var(--sb-brand-blue);
  color: white;
}

.result-title {
  text-align: left;
  font-weight: 500;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: -0.015em;

  color: #000000;
}

.radio-group-btns {
  display: flex;
}


.result-wrapper {
  margin-top: 16px;
}

.result-placeholder {
  text-align: left;

  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  letter-spacing: -0.015em;
  color: rgba(0, 0, 0, 0.15);
}

.result {
  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  letter-spacing: -0.015em;
  color: #000000;
}

.result-text-wrapper {
  padding: 8px 0;
  text-align: left;
}
</style>
