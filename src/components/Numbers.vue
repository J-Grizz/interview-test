<template>
  <div>
    <!-- Using the num as key seems like bad practice, id rather use something like vue-uuid for key gen -->
    <div class="num" ref="nums" :id="'num-'+num" v-for="num in gen_nums()" :key="'num-'+num" @mouseover="hov(num)" @mouseout="reset">
      <span>{{num}}</span>
    </div>
  </div>
</template>

<script>
export default {
  // Added name of component
  name: 'Numbers',
  // Use props over the atrocity of $this.parent
  props: ['limit'],
  data()
  {
    return {
      nums: []
    }
  },
  methods: {
    // Function n is badly named
    gen_nums()
    {
      let nums = [];
      for(var i = 0; i < this.limit; i++)
      {
        // .push is more performant than using spread operator, O(1) vs O(N)
        nums.push(i);
      }
      return nums.sort(() => Math.random() - 0.5);
    },


    hov(hovNum)
    {
      // Better practice to use refs over query selector
      const nums = this.$refs.nums;
      // Cleaned this guy up a bit
      nums.forEach(num => {
        if(hovNum % num.textContent.trim() === 0) {
          num.classList.add('active');
        }
      });
    },

    reset()
    {
      // Better practice to use refs over query selector
      const nums = this.$refs.nums;
      nums.forEach(num => num.classList.remove('active'))
    }
  }
}
</script>

<style scoped>
	.num {
		display: inline-block;
		padding: 5px;
		background-color: lightgrey;
		margin: 5px;
	}

	.active {
		background-color: red;
	}
</style>
