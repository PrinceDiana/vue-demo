<template>
	<div class="hello">
		<h1>{{ msg }}</h1>
		<h2>Essential Links</h2>

		<ul>
            <li v-for="(item, index) in msgList" :key="index">
                <span :class="{ 'even-text': (index % 2 == 0) }">
                    No.{{index + 1}}: {{item.name}}
                </span>
            </li>
        </ul>

		<a @click="updateFromChild($event, 'man')">子组件修改消息</a>

		<a @click="updateMessage">子组件修改message</a>

        <router-link :to="{ name: 'DiffDom' }">Go Diff Page</router-link>
	</div>
</template>

<script>
import IntervalMixin from '../mixins/interval';

export default {
    props: {
		message: {
			type: String,
			default: ''
		},
        msgList: {
            type: Array,
		},
		msgLog: {
			type: Function
		}
    },
    mixins: [IntervalMixin],
    name: 'hello',
    data() {
        return {
			msg: '我是子组件',
            privateMsg: '子组件的私有消息',
            intervalId: null, // 定时器序号
        };
	},
	created() {
		// this.msgLog(() => {
		// 	console.log(this.privateMsg);
        // });

        this.intervalId = setInterval(() => {
            console.log('interval msg from hello page');
        }, 1000);
	},
	methods: {
		updateFromChild(event, type) {
			const msg = {
                name: Date.now()
            }

			this.$set(this.msgList, 0, msg);
		},
		updateMessage() {
			this.$emit('updateMessage', '我是修改过的message');
		}
	}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
