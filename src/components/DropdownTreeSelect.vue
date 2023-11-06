<script setup>
import {onMounted, ref, toRaw} from 'vue';

const props = defineProps(['items']);
const emit = defineEmits(['change-selections']);

const selections = ref([]);
let leafElementsSelections = {};
let treeDomElement = null;

onMounted(() => {
  treeDomElement = document.querySelector('.tree');

  let rootTreeElement = document.createElement('div');

  createTree(props.items, rootTreeElement);
  treeDomElement.appendChild(rootTreeElement);
});

function createTree(items, element) {
  for (const item of items) {
    if (item.groupLabel) {
      const nestedGroupElement = document.createElement('div');
      nestedGroupElement.classList.add('group');
      nestedGroupElement.style.paddingLeft = `1rem`;
      nestedGroupElement.addEventListener('group-nested-leaf-change', handleNestedLeafChange);

      const plusElement = document.createElement('span');
      plusElement.innerText = '+';
      plusElement.style.paddingRight = `0.1rem`;
      plusElement.addEventListener('click', handleToggleChildrenVisibility);
      nestedGroupElement.appendChild(plusElement);

      const checkboxElement = document.createElement('input');
      checkboxElement.type = 'checkbox';
      checkboxElement.addEventListener('change', handleGroupChange);


      nestedGroupElement.appendChild(checkboxElement);

      const nestedGroupNameElement = document.createElement('span');
      nestedGroupNameElement.innerText = item.groupLabel;
      nestedGroupNameElement.style.paddingLeft = `0.1rem`;

      nestedGroupElement.appendChild(nestedGroupNameElement);

      element.appendChild(nestedGroupElement);

      const nestedGroupChildrenElement = document.createElement('div');
      nestedGroupChildrenElement.classList.add('group-children');
      nestedGroupElement.appendChild(nestedGroupChildrenElement);


      createTree(item.children, nestedGroupChildrenElement);
    } else {
      const leafElement = document.createElement('div');
      leafElement.classList.add('leaf');
      leafElement.style.paddingLeft = `1.7rem`;

      const checkboxElement = document.createElement('input');
      checkboxElement.type = 'checkbox';
      leafElement.appendChild(checkboxElement);

      const leafElementName = document.createElement('span');
      leafElementName.innerText = item.label;
      leafElementName.style.paddingLeft = '0.1rem';
      leafElementName.setAttribute('value', item.value);
      leafElement.appendChild(leafElementName);

      checkboxElement.addEventListener('change', handleChange);

      element.appendChild(leafElement);
    }
  }
}

function handleChange(event) {
  const spanElement = event.currentTarget.parentNode.childNodes[1];
  const label = spanElement.textContent;
  const value = spanElement.getAttribute('value');
  if (event.currentTarget.checked) {
    selections.value.push({label, value});
    leafElementsSelections[value] = event.currentTarget.parentNode;
  }
  else {
    removeSelection(value);
  }
  event.currentTarget.dispatchEvent(groupNestedLeafChangeEvent);

  emit('change-selections', toRaw(selections.value));
}

function removeSelection(value) {
  selections.value = toRaw(selections.value).filter(selection => selection.value !== value );
}

function handleNestedLeafChange(event) {
  const groupChildrenArray = Array.from(event.currentTarget.childNodes[3].childNodes);
  const filteredGroupChildrenArray = groupChildrenArray.filter((groupChild) => {
    return ( (groupChild.classList.contains('leaf')) && (groupChild.childNodes[0].checked) ||
        (groupChild.classList.contains('group')) && (groupChild.childNodes[1].checked) )
  });
  if (filteredGroupChildrenArray.length < groupChildrenArray.length)
    event.currentTarget.childNodes[1].checked = false
  else if (filteredGroupChildrenArray.length === groupChildrenArray.length)
    event.currentTarget.childNodes[1].checked = true;
}

function handleToggleChildrenVisibility(event) {
  const groupChildrenElement = event.currentTarget.parentNode.childNodes[3];
  groupChildrenElement.style.display === 'none' ? groupChildrenElement.style.display = 'block' : groupChildrenElement.style.display = 'none';
}

const groupNestedLeafChangeEvent = new Event('group-nested-leaf-change', {bubbles: true});

function allGroupDeselected(element) {
  const arr = Array.from(element.childNodes);

  const selectedLeafElements = arr.filter((childNode, index) => {
    return ((index >= 3) && (childNode.childNodes[0].checked))
  });
  return selectedLeafElements.length === 0;
}

function handleGroupChange(event) {
  toggleNestedSelections(event.currentTarget.checked, event.currentTarget.parentNode.childNodes);
}

function toggleNestedSelections(checked, childNodes) {
  for (const childElement of childNodes) {
    if (childElement.className === 'leaf') {
      const checkBoxElement =  childElement.childNodes[0];
      if (checkBoxElement.checked === checked)
        continue;
      const spanElement = checkBoxElement.parentNode.childNodes[1];
      const value = spanElement.getAttribute('value');
      if (checked) {
        const label = spanElement.textContent;
        selections.value.push({label, value});
        leafElementsSelections[value] = checkBoxElement.parentNode;
      }
      else
        removeSelection(value);
      checkBoxElement.checked = checked;
    } else if (childElement.className === 'group') {
      const checkBoxElement = childElement.childNodes[1];
      checkBoxElement.checked = checked;
      toggleNestedSelections(checked, childElement.childNodes[3].childNodes);
    } else if (childElement.className === 'group-children') {
      toggleNestedSelections(checked, childElement.childNodes);
    }
  }
}

function handleRemoveSelection(event) {
  const index = event.currentTarget.parentNode.attributes['data-index'].value;
  const value = event.currentTarget.parentNode.attributes['data-value'].value;
  selections.value.splice(index, 1);

  const leafElementSelected = leafElementsSelections[value];
  leafElementSelected.childNodes[0].checked = false;
  leafElementSelected.dispatchEvent(groupNestedLeafChangeEvent);
}

function toggleTreeVisibility(event) {
  if (event.currentTarget.innerHTML === '▼') {
    event.currentTarget.innerHTML = '▲'
    treeDomElement.style.visibility = 'visible';
  } else {
    event.currentTarget.innerHTML = '▼'
    treeDomElement.style.visibility = 'hidden';
  }
}

</script>

<template>
  <!--  <div>-->
  <div class="selections">
      <span v-for="(selection, index) in selections" class="item-selected" :data-index="index" :data-value="selection.value">
        {{selection.label}}
        <span class="remove-item" @click="handleRemoveSelection">x</span>
      </span>

    <div class="arrow-container">
      <div class="arrow-vertical-container">
        <span class="arrow" @click="toggleTreeVisibility">&#9660;</span>
      </div>
    </div>
  </div>

  <div class="tree" style="visibility: hidden"/>

</template>


<style scoped>

.selections {
  border: solid 1px grey;
  padding: 1rem;
  width: 30rem;
  overflow: scroll;
  flex: 0 0 6rem;
  display: flex;
  gap: 1rem;
}

.item-selected {
  padding: 0.5rem;
  margin: 0.5rem;
  background-color: grey;
  white-space: nowrap;
}

.remove-item {
  color: #34495e6e;
  font-size: 1.2em;
  padding-left: 0.2rem;
}

.remove-item:hover {
  cursor: pointer;
}

.arrow-container {
  flex: 1;
  display: flex;
  justify-content: end;
}

.arrow-vertical-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.arrow:hover {
  cursor: pointer;
}

.tree {
  flex: 2;
}

</style>
