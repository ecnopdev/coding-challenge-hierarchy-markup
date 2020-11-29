<template>
  <div id="app">
    <div class="people-container" v-html="heirarchyMarkup"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      people: [
        { id: 100, name: "Allan", manager_id: 150 },
        { id: 220, name: "Martin", manager_id: 100 },
        { id: 150, name: "Jamie", manager_id: null },
        { id: 275, name: "Alex", manager_id: 100 },
        { id: 400, name: "Steve", manager_id: 150 },
        { id: 190, name: "David", manager_id: 400 },
      ],
      heirarchy: {},
      heirarchyMarkup: "",
    };
  },
  mounted: function () {
    this.heirarchy = this.transformToHeirarchical();
    this.displayHeirarchyOnConsole(this.heirarchy["no_manager"], "");
    this.displayHeirarchyOnPage(this.heirarchy["no_manager"], 0);
  },
  methods: {
    transformToHeirarchical() {
      let heirarchicalData = {};

      this.people.forEach((person) => {
        if (!person.manager_id) {
          if (heirarchicalData["no_manager"]) {
            heirarchicalData["no_manager"].push(person);
          } else {
            heirarchicalData["no_manager"] = [person];
          }
        } else {
          if (heirarchicalData[person.manager_id]) {
            heirarchicalData[person.manager_id].push(person);
          } else {
            heirarchicalData[person.manager_id] = [person];
          }
        }
      });

      return heirarchicalData;
    },
    displayHeirarchyOnConsole(topLevel, indent) {
      topLevel.forEach((person) => {
        console.log(`${indent} ${person.name}`);

        if (this.heirarchy[person.id]) {
          this.displayHeirarchyOnConsole(
            this.heirarchy[person.id],
            `${indent}       |`
          );
        }
      });
    },
    displayHeirarchyOnPage(topLevel, indent) {
      topLevel.forEach((person) => {
        this.heirarchyMarkup += `<div class="person-row"><span class="indent-${indent}">${person.name}</span></div>`;

        if (this.heirarchy[person.id]) {
          this.displayHeirarchyOnPage(this.heirarchy[person.id], indent + 1);
        }
      });
    },
  },
};
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.people-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: auto;
}

.person-row {
  display: flex;
  width: 100%;
  border: 1px solid black;
  border-collapse: collapse;
  padding: 10px;
}

.person-row span {
  font-weight: bold;
}

.indent-1 {
  margin-left: 70px;
}

.indent-2 {
  margin-left: 140px;
}

.indent-3 {
  margin-left: 210px;
}
</style>
