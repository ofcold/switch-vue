<template>
	<span :class="warpperClasses" @click="toggle">
		<input type="hidden" :name="name" :value="currentValue">
		<span :class="innerClasses">
			<slot name="open" v-if="currentValue === trueValue"></slot>
			<slot name="close" v-if="currentValue === falseValue"></slot>
		</span>
	</span>
</template>
<script>
	export default {
		name: 'OfcoldSwitch',
		props: {
			value: {
				type: [String, Number, Boolean],
				default: false
			},
			trueValue: {
				type: [String, Number, Boolean],
				default: true
			},
			falseValue: {
				type: [String, Number, Boolean],
				default: false
			},
			disabled: {
				type: Boolean,
				default: false
			},
			size: {
				validator (value) {
					return (new Array('sm', 'lg')).indexOf(value) >= 0;
				}
			},
			name: {
				type: String
			}
		},
		computed: {
			warpperClasses () {
				return [
					'ofcold-switch',
					{
						'ofcold-switch-checked': this.value === this.trueValue,
						'ofcold-switch-disabled': this.disabled,
						[`ofcold-switch-${this.size}`]: !!this.size
					}
				];
			},
			innerClasses () {
				return 'ofcold-switch-inner';
			}
		},
		methods: {
			toggle () {
				if (this.disabled) {
					return false;
				}

				const checked = this.value === this.trueValue ? this.falseValue : this.trueValue;
				this.value = checked;
				this.$emit('input', checked);
			}
		},
		watch: {
			value (val) {
				if (val !== this.trueValue && val !== this.falseValue) {
					throw 'Ofcold Switch Value should be trueValue or falseValue.';
				}
				this.value = val;
			}
		}
	};
</script>

<style lang="scss" scoped>
.ofcold-switch {
	display: inline-block;
	width: 59px;
	height: 28px;
	border-radius: 28px;
	vertical-align: middle;
	border: 1px solid #ccc;
	background-color: #ccc;
	position: relative;
	cursor: pointer;
	user-select: none;
	transition: all .2s ease-in-out;

	&-inner {
		color: #fff;
		font-size: .875rem;
		position: absolute;
		left: 28px;

		i {
			width: 12px;
			height: 12px;
			text-align: center;
		}
	}

	&:after {
		content: '';
		width: 24px;
		height: 24px;
		border-radius: 24px;
		background-color: #fff;
		position: absolute;
		left: 1px;
		top: 1px;
		cursor: pointer;
		transition: left .2s ease-in-out, width .2s ease-in-out;
	}

	&:active:after {
		width: 28px;
	}

	&:focus {
		box-shadow: 0 0 0 2px fade(#21272e, 20%);
		outline: 0;
	}

	&:focus:hover {
		box-shadow: none;
	}

	&-checked {
		border-color: #21272e;
		background-color: #21272e;

		.ofcold-switch__-inner {
			left: 8px;
		}

		&:after {
			left: 32px;
		}

		&:active:after {
			left: 19px;
		}
	}

	&-disabled {
		cursor: not-allowed;
		background: #f3f3f3;
		border-color: #f3f3f3;

		&:after {
			background: #ccc;
			cursor: not-allowed;
		}

		.ofcold-switch__-inner {
			color: #ccc;
		}
	}

	&-sm {
		width: 24px;
		height: 12px;
		line-height: 10px;
		&:after {
			width: 10px;
			height: 10px;
			top: 0;
			left: 0;
		}
		&:active:after {
			width: 14px;
		}
	}

	&-sm.ofcold-switch-checked:after {
		left: 12px;
	}

	&-sm:active.ofcold-switch-checked:after {
		left: 8px;
	}

	&-lg{
		width: 99px;
		height: 44px;
		&:after {
			width: 36px;
			height: 36px;
			border-radius: 36px;
			left: 3px;
			top: 3px;
		}
	}

	&-lg.ofcold-switch-checked:after {
		left: 58px;
	}

	&-lg:active.ofcold-switch-checked:after {
		left: 58px;
	}
}
</style>
