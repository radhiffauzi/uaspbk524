<template>
     <div class="red">
      <div class="black">
        <h1> MY APP <span></span></h1>
      </div>
      <div class="list">
        <div class="li1">
          <div class="li2">
            <h2 class="addtitle">Add new todo...</h2>
            <input v-model="todo" type="text" class="box" @keyup.enter="add" />
          </div>
          <div>
            <button class="button-20" @click="add">ADD</button>
          </div>
        </div>
        <small class="titillium-web-regular">Total TODO : {{ totalTODO }}</small>
        <ul class="list-group mt-3">
          <li class="list-group-item d-flex justify-content-between align-items-center" v-for="(item, index) in todos" :key="index" :class="{ 'completed': item.completed }">
            <span v-if="!item.completed">{{ item.text }}</span>
            <span v-else class="done">{{ item.text }}</span>
            <div>
              <button class="button-29" @click="remove(index)">X</button>
              <button class="button-28" @click="toggleCompletion(index)">{{ item.completed ? 'Undone' : 'Done' }}</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
</template>

