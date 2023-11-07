# Vue Dropdown Tree Select

![NPM License](https://img.shields.io/npm/l/vue-dropdown-tree-select)

**Lightweight** component for displaying hierarchical data **< 15k** 😎 <br>
Emits selections to parent component

![dropdown-tree-select](https://github.com/gshohat/vue-dropdown-tree-select/assets/91323932/01cdce2c-7648-46e3-a3ab-d0d0615cd205)


## Usage

`npm i vue-dropdown-tree-select`

```
<script setup>
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

