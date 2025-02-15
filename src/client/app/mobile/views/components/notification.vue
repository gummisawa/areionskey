<template>
<div class="mk-notification">
	<div class="notification reaction" v-if="notification.type == 'reaction'">
		<mk-avatar class="avatar" :user="notification.user"/>
		<div>
			<header>
				<mk-reaction-icon :reaction="notification.reaction" :custom-emojis="notification.note.emojis"/>
				<router-link class="name" :to="notification.user | userPage"><mk-user-name :user="notification.user"/></router-link>
				<mk-time :time="notification.createdAt"/>
			</header>
			<router-link class="note-ref" :to="notification.note | notePage" :title="getNoteSummary(notification.note)">
				<fa icon="quote-left"/>
					<mfm :text="getNoteSummary(notification.note)" :plain="true" :nowrap="true" :custom-emojis="notification.note.emojis"/>
				<fa icon="quote-right"/>
			</router-link>
		</div>
	</div>

	<div class="notification renote" v-if="notification.type == 'renote'">
		<mk-avatar class="avatar" :user="notification.user"/>
		<div>
			<header>
				<fa icon="retweet"/>
				<router-link class="name" :to="notification.user | userPage"><mk-user-name :user="notification.user"/></router-link>
				<mk-time :time="notification.createdAt"/>
			</header>
			<router-link class="note-ref" :to="notification.note | notePage" :title="getNoteSummary(notification.note.renote)">
				<fa icon="quote-left"/>
					<mfm :text="getNoteSummary(notification.note.renote)" :plain="true" :nowrap="true" :custom-emojis="notification.note.renote.emojis"/>
				<fa icon="quote-right"/>
			</router-link>
		</div>
	</div>

	<div class="notification follow" v-if="notification.type == 'follow'">
		<mk-avatar class="avatar" :user="notification.user"/>
		<div>
			<header>
				<fa icon="user-plus"/>
				<router-link class="name" :to="notification.user | userPage"><mk-user-name :user="notification.user"/></router-link>
				<mk-time :time="notification.createdAt"/>
			</header>
			<span class="notify-info">{{ $t('youGotNewFollower') }}</span>
		</div>
	</div>

	<div class="notification followRequest" v-if="notification.type == 'receiveFollowRequest'">
		<mk-avatar class="avatar" :user="notification.user"/>
		<div>
			<header>
				<fa icon="user-clock"/>
				<router-link class="name" :to="notification.user | userPage"><mk-user-name :user="notification.user"/></router-link>
				<mk-time :time="notification.createdAt"/>
			</header>
			<span class="notify-info">{{ $t('receiveFollowRequest') }}</span>
		</div>
	</div>

	<div class="notification pollVote" v-if="notification.type == 'pollVote'">
		<mk-avatar class="avatar" :user="notification.user"/>
		<div>
			<header>
				<fa icon="chart-pie"/>
				<router-link class="name" :to="notification.user | userPage"><mk-user-name :user="notification.user"/></router-link>
				<mk-time :time="notification.createdAt"/>
			</header>
			<router-link class="note-ref" :to="notification.note | notePage" :title="getNoteSummary(notification.note)">
				<fa icon="quote-left"/>
					<mfm :text="getNoteSummary(notification.note)" :plain="true" :nowrap="true" :custom-emojis="notification.note.emojis"/>
				<fa icon="quote-right"/>
			</router-link>
		</div>
	</div>

	<template v-if="notification.type == 'quote'">
		<mk-note :note="notification.note"/>
	</template>

	<template v-if="notification.type == 'reply'">
		<mk-note :note="notification.note"/>
	</template>

	<template v-if="notification.type == 'mention'">
		<mk-note :note="notification.note"/>
	</template>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import i18n from '../../../i18n';
import getNoteSummary from '../../../../../misc/get-note-summary';

export default Vue.extend({
	i18n: i18n('mobile/views/components/notification.vue'),

	props: ['notification'],
	data() {
		return {
			getNoteSummary
		};
	},
});
</script>

<style lang="stylus" scoped>
.mk-notification

	&.wide
		> .notification
			@media (min-width 350px)
				font-size 14px

			@media (min-width 500px)
				font-size 16px

			@media (min-width 600px)
				padding 24px 32px

			> .avatar
				@media (min-width 500px)
					width 42px
					height 42px

			> div
				@media (min-width 500px)
					width calc(100% - 42px)

	> .notification
		padding 16px
		font-size 12px
		overflow-wrap break-word

		&:after
			content ""
			display block
			clear both

		> .avatar
			display block
			float left
			width 36px
			height 36px
			border-radius 6px

		> div
			float right
			width calc(100% - 36px)
			padding-left 8px

			> header
				display flex
				align-items baseline
				white-space nowrap

				[data-icon], .mk-reaction-icon
					margin-right 4px

				> .name
					text-overflow ellipsis
					white-space nowrap
					min-width 0
					overflow hidden

				> .mk-time
					margin-left auto
					color var(--noteHeaderInfo)
					font-size 0.9em

			> .note-preview
				color var(--noteText)

			> .note-ref
				color var(--noteText)
				display inline-block
				width: 100%
				overflow hidden
				white-space nowrap
				text-overflow ellipsis

				[data-icon]
					display inline-block
					font-size: 50%
					opacity: 0.5
					vertical-align: super

				[data-icon]:first-child
					margin-left: 4px
					margin-right: 4px

				[data-icon]:last-child
					margin-left: 4px

			> .notify-info
				color var(--noteText)
				display inline-block
				width: 100%
				opacity: 0.5
				overflow hidden
				white-space nowrap

		&.reaction
			> div > header
				align-items normal

		&.reply, &.mention
			> div > header [data-icon]
				color #007aff

		&.renote, &.quote
			> div > header [data-icon]
				color #36d298

		&.follow
			> div > header [data-icon]
				color #36aed2

		&.followRequest, &.pollVote
			> div > header [data-icon]
				color #5e7c8c

</style>
