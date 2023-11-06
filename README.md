# Vue Dropdown Tree Select

![NPM License](https://img.shields.io/npm/l/vue-dropdown-tree-select)

**Lightweight** component for displaying hierarchical data **< 15k** 😎 <br>
Emits selections to parent component

![dropdown-tree-select](https://private-user-images.githubusercontent.com/91323932/280632865-f02d53c2-1551-4dc1-a2bb-c3ef8f377cfc.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE2OTkyNTgxNTMsIm5iZiI6MTY5OTI1Nzg1MywicGF0aCI6Ii85MTMyMzkzMi8yODA2MzI4NjUtZjAyZDUzYzItMTU1MS00ZGMxLWEyYmItYzNlZjhmMzc3Y2ZjLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMDYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTA2VDA4MDQxM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTE4MjkyNzBjMTZjNmI3ZGYwNjE1YmE2OTg4MTU5NDA3NDljNGQ4Mjc3YTAzYWZiODg2NWJkOWNjMzE1ZTM4ZjYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.S-KHFLWCSexBeSOIerWiSER6X1BBxOQL-VPf4inj1pI)

## Usage

`npm i vue-dropdown-tree-select`

```
<script setup>π
import DropdownTreeSelect from 'vue-dropdown-tree-select';
import 'vue-dropdown-tree-select/dist/style.css';

const items = [
  { groupLabel: 'Frontend Developer',
    children: [
      {groupLabel: 'Vue',
        children: [ { label:'Options Api', value: 'optionsApi'},
          { label: 'Composition Api', value: 'compositionApi' }]},
      { label: 'React', value: 'react'},
      { label: 'Angular', value: 'angular'}
    ]},
  { groupLabel: 'Backend Developer',
    children: [
      {label: 'Bun', value: 'bun' },
      { label: 'Deno', value: 'deno' },
      { label:'Node', value: 'node' }
    ]},
];

function handleChange(selections) {
  //todo selections = [ {label '', value: ''}, .... ];
  // console.log(selections);
}

</script>


<template>
<DropdownTreeSelect :items="items" @change-selections="handleChange"/>
</template
```


## Contact
Feel free to ping me 💫
<br>
connect@giladshohat.com

[giladshohat.com](https://giladshohat.com)

