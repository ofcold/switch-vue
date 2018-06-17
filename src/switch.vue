<template>
	<span :class="wrapClasses" @click="toggle">
		<input type="hidden" :name="name" :value="currentValue">
		<span :class="innerClasses">
			<slot name="open" v-if="currentValue === trueValue"></slot>
			<slot name="close" v-if="currentValue === falseValue"></slot>
		</span>
	</span>
</template>
<script>
	const prefixClass = 'ofcold-switch__';
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
					return (new Array('sm', 'md', 'lg')).indexOf(value) >= 0;
				}
			},
			name: {
				type: String
			}
		},
		data () {
			return {
				currentValue: this.value
			};
		},
		computed: {
			wrapClasses () {
				return [
					`${prefixClass}`,
					{
						[`${prefixClass}-checked`]: this.currentValue === this.trueValue,
						[`${prefixClass}-disabled`]: this.disabled,
						[`${prefixClass}-${this.size}`]: !!this.size
					}
				];
			},
			innerClasses () {
				return `${prefixClass}-inner`;
			}
		},
		methods: {
			toggle () {
				if (this.disabled) {
					return false;
				}
				const checked = this.currentValue === this.trueValue ? this.falseValue : this.trueValue;
				this.currentValue = checked;
				this.$emit('input', checked);
				this.$emit('on-change', checked);
			}
		},
		watch: {
			value (val) {
				if (val !== this.trueValue && val !== this.falseValue) {
					throw 'Value should be trueValue or falseValue.';
				}
				this.currentValue = val;
			}
		}
	};
</script>

<style lang="scss" scoped>
.ofcold-switch__-switch {
	display: inline-block;
	width: 48px;
	height: 24px;
	line-height: 22px;
	border-radius: 24px;
	vertical-align: middle;
	border: 1px solid #ccc;
	background-color: #ccc;
	position: relative;
	cursor: pointer;
	user-select: none;
	transition: all .2s ease-in-out;

	&-inner {
		color: #fff;
		font-size: $font-size-sm;
		position: absolute;
		left: 25px;

		i {
			width: 12px;
			height: 12px;
			text-align: center;
		}
	}

	&:after {
		content: '';
		width: 20px;
		height: 20px;
		border-radius: 20px;
		background-color: #fff;
		position: absolute;
		left: 1px;
		top: 1px;
		cursor: pointer;
		transition: left .2s ease-in-out, width .2s ease-in-out;
	}

	&:active:after {
		width: 26px;
	}

	&:focus {
		box-shadow: 0 0 0 2px fade($gray-dark, 20%);
		outline: 0;
	}

	&:focus:hover {
		box-shadow: none;
	}

	&-small {
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

	&-small.ofcold-switch__-switch-checked:after {
		left: 12px;
	}

	&--small:active.ofcold-switch__-switch-checked:after {
		left: 8px;
	}

	&-large{
		width: 60px;
		&:active:after {
			width: 26px;
		}
	}

	&-large:active:after {
		width: 32px;
	}

	&-large.ofcold-switch__-switch-checked:after {
		left: 37px;
	}

	&-large:active.ofcold-switch__-switch-checked:after {
		left: 25px;
	}

	&-checked {
		border-color: $gray-dark;
		background-color: $gray-dark;

		.ofcold-switch__-switch-inner {
			left: 8px;
		}

		&:after {
			left: 25px;
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

		.ofcold-switch__-switch-inner {
			color: #ccc;
		}
	}
}
</style>