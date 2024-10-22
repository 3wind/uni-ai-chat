<template>
	<view class="app-container">
		<view class="input-area">
			<uni-easyinput type="textarea" v-model="userInput" placeholder="请输入您的问题..." suffixIcon="paperplane-filled"
				@iconClick="submitQuestion"></uni-easyinput>
		</view>
		<view class="chat-list" ref="chatList">
			<view v-for="(message, index) in chatMessages" :key="index" :class="['message', message.type]">
				{{ message.text }}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInput: '',
				chatMessages: []
			};
		},
		methods: {
			submitQuestion() {
				if (this.userInput.trim()) {
					this.chatMessages.push({
						text: this.userInput,
						type: 'user'
					});
					this.userInput = '';

					// 模拟向后端发送请求
					setTimeout(() => {
						const response = '这是AI的回答。'; // 这里应该替换为实际的API调用
						this.chatMessages.push({
							text: response,
							type: 'ai'
						});

						// 滚动到底部
						this.scrollToBottom();
					}, 500);
				}
			},
			scrollToBottom() {
				const chatList = this.$refs.chatList;
				if (chatList) {
					chatList.scrollTop = chatList.scrollHeight;
				}
			}
		}
	};
</script>

<style lang="scss">
	.app-container {
		display: flex;
		flex-direction: column;
		height: 100vh;
		gap: 16rpx;

		.input-area {
			border-top: 1px solid #ddd;
			background: white;
			position: fixed;
			bottom: 0;
			padding: 8rpx 0;
			width: 100%;

			.easyinput--uni-easyinput {
				padding: 16rpx;
				width: auto;
				flex: 1;
				border-radius: 8px;
			}
		}

		.chat-list {
			overflow-y: auto;
			padding: 20rpx;
			flex: 1;
			display: flex;
			flex-direction: column;
		}

		.message {
			padding: 10rpx;
			margin: 5rpx 0;
			border-radius: 10px;
			max-width: 70%;
		}

		.message.user {
			align-self: flex-end;
			background: #dcf8c6;
		}

		.message.ai {
			align-self: flex-start;
			background: #ebebeb;
		}
	}
</style>