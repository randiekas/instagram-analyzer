<template>
    <div class="grey lighten-4 fill-height mb-16">
		<div class="primary pb-16">
            <v-container>
                <div :class="`mt-6 text-h6 d-flex justify-space-between white--text`">
                    <div class="d-flex">
                        <v-avatar size="62">
                            <v-img 
                                :src="user.profile_pic_url_hd"/>
                        </v-avatar>
                        <div class="ml-2">
                            <p class="text-h4 mb-0">@{{ username }}</p>
                            <p class="subtitle-2">{{ user.biography }}</p>
                        </div>
                    </div>
                    <v-btn 
                        small 
                        class="white"
                        to="/apps/beranda">
                        <v-icon left>
                            mdi-chevron-left
                        </v-icon>
                        Kembali
                    </v-btn>
                </div>
                <v-card class="mt-4 text-center">
                    <v-row>
                        <v-col>
                            <p class="text-h4 mb-0">{{ interaction }}</p>
                            <p class="text-overline mb-0">Interactions</p>
                        </v-col>
                        <v-col>
                            <p class="text-h4 mb-0">{{ user.edge_owner_to_timeline_media.count }}</p>
                            <p class="text-overline mb-0">Posts</p>
                        </v-col>
                        <v-col>
                            <p class="text-h4 mb-0">{{ user.edge_followed_by.count }}</p>
                            <p class="text-overline mb-0">Followers</p>
                        </v-col>
                        <v-col>
                            <p class="text-h4 mb-0">{{ user.edge_follow.count }}</p>
                            <p class="text-overline mb-0">Following</p>
                        </v-col>
                    </v-row>
                </v-card>
            </v-container>
        </div>
        <v-container class="mt-n16">
            <v-tabs v-model="tab">
                <v-tab>Analisis Umum</v-tab>
                <v-tab>Analisis Followers</v-tab>
            </v-tabs>
            
            <template v-if="tab==0">
                <v-card class="mb-4 mt-4 d-flex align-items-center pr-2" style="align-items:center">
                    <div>
                        <v-card-title>
                            Influencer Score
                        </v-card-title>
                        <v-card-subtitle>Score represents influence relative performance in key metrics: Sum of interactions last 12 posts/followers</v-card-subtitle>
                    </div>
                    <v-spacer/>
                    <p class="mb-0 text-h3">
                        {{ score }}%
                    </p>
                </v-card>
                <v-card class="mb-4">
                    <div class="d-flex align-items-center pr-2" style="align-items:center">
                        <div>
                            <v-card-title>
                                Latest Posts
                            </v-card-title>
                        </div>
                        <v-spacer/>
                        <p class="mb-0 text-h3">
                            {{ user.edge_owner_to_timeline_media.edges.length }}
                        </p>
                    </div>
                    <v-card-text>
                        <v-row>
                            <v-col 
                                v-for="(item, index) in user.edge_owner_to_timeline_media.edges"
                                :key="index"
                                md="3">
                                <v-card outlined>
                                    <v-img 
                                        :src="item.node.thumbnail_src"/>
                                    
                                    <v-list dense class="py-0">
                                        <v-list-item>
                                            <v-list-item-title>Like</v-list-item-title>
                                            <v-spacer/>
                                            <v-list-item-action-text>{{ item.node.edge_liked_by.count }}</v-list-item-action-text>
                                        </v-list-item>
                                        <v-divider/>
                                        <v-list-item>
                                            <v-list-item-title>Comments</v-list-item-title>
                                            <v-spacer/>
                                            <v-list-item-action-text>{{ item.node.edge_media_to_comment.count }}</v-list-item-action-text>
                                        </v-list-item>
                                        <v-divider/>
                                        <v-list-item>
                                            <v-list-item-title>Interaksi</v-list-item-title>
                                            <v-spacer/>
                                            <v-list-item-action-text>{{ item.node.edge_liked_by.count+item.node.edge_media_to_comment.count }}</v-list-item-action-text>
                                        </v-list-item>
                                    </v-list>
                                
                                </v-card>
                            </v-col>
                        </v-row>
                    </v-card-text>
                </v-card>
                <v-card class="mb-4 pr-2">
                    <div>
                        <v-card-title>
                            Location Tags
                        </v-card-title>
                        <v-list dense class="py-0">
                            <template
                                v-for="(item, index) in location">
                                <v-list-item
                                    :key="index">
                                    <v-list-item-content>
                                        <v-list-item-title>{{ item.name }}</v-list-item-title>
                                        <v-progress-linear :value="item.total/location[0].total*100"></v-progress-linear>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                        <v-list-item-action-text>{{ item.total }}</v-list-item-action-text>
                                    </v-list-item-action>
                                </v-list-item>
                                <v-divider
                                    :key="`line-${index}`"/>
                            </template>
                        </v-list>
                    </div>
                </v-card>
                <v-card class="mb-4 pr-2">
                    <div>
                        <v-card-title>
                            Tags
                        </v-card-title>
                        <v-list dense class="py-0">
                            <template
                                v-for="(item, index) in tags">
                                <v-list-item
                                    :key="index">
                                    <v-list-item-avatar>
                                        <v-avatar size="24" class="mr-2">
                                            <v-img 
                                                :src="item.profile_pic_url"/>
                                        </v-avatar>
                                    </v-list-item-avatar>
                                    <v-list-item-content>
                                        <v-list-item-title>
                                            {{ item.username }}
                                        </v-list-item-title>
                                        <v-progress-linear :value="item.total/tags[0].total*100"></v-progress-linear>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                        <v-list-item-action-text>
                                            {{ item.total }}
                                        </v-list-item-action-text>
                                    </v-list-item-action>
                                </v-list-item>
                                <v-divider
                                    :key="`divider-${index}`"/>
                            </template>
                        </v-list>
                    </div>
                </v-card>
                <v-card class="mb-4 pr-2">
                    <div>
                        <v-card-title>
                            Profiles Mentioning @{{username}}
                        </v-card-title>
                        <v-list dense class="py-0">
                            <template
                                v-for="(item, index) in mention">
                                <v-list-item
                                    :key="index">
                                    <v-list-item-content>
                                        <v-list-item-title>{{ item.username }}</v-list-item-title>
                                        <v-progress-linear :value="item.total/mention[0].total*100"></v-progress-linear>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                        <v-list-item-action-text>{{ item.total }}</v-list-item-action-text>
                                    </v-list-item-action>
                                </v-list-item>
                                <v-divider
                                    :key="`line-${index}`"/>
                            </template>
                        </v-list>
                    </div>
                </v-card>
                <v-card class="mb-4 pr-2">
                    <div>
                        <v-card-title>
                            People Mention By @{{username}}
                        </v-card-title>
                        <v-list dense class="py-0">
                            <template
                                v-for="(item, index) in user.imention.buckets">
                                <v-list-item
                                    :key="index">
                                    <v-list-item-content>
                                        <v-list-item-title>{{ item.key }}</v-list-item-title>
                                        <v-progress-linear :value="item.doc_count/user.imention.buckets[0].doc_count*100"></v-progress-linear>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                        <v-list-item-action-text>{{ item.doc_count }}</v-list-item-action-text>
                                    </v-list-item-action>
                                </v-list-item>
                                <v-divider
                                    :key="`line-${index}`"/>
                            </template>
                        </v-list>
                    </div>
                </v-card>
                <v-card class="mb-4">
                    <v-card-title>
                        Most Used Hashtag
                    </v-card-title>
                    <v-card-text>
                        <vue-word-cloud
                            style="
                                height: 480px;
                                width: 100%;"
                            :words="hashtag"
                            :color="([, weight]) => weight > 10 ? 'DeepPink' : weight > 5 ? 'RoyalBlue' : 'Indigo'"
                            font-family="Roboto"/>
                    
                    </v-card-text>
                </v-card>
            
            </template>
            <template v-else>
                <v-card class="mb-4 mt-4 text-center">
                    <v-card-title>
                        #1 Mining Followers
                        <v-spacer/>
                        {{ followersMined }}/{{ user.edge_followed_by.count }}
                        <v-btn @click="startMiningFollowers" class="ml-2">Tambang</v-btn>
                        <v-btn @click="checkStatusMiningFollowers" class="ml-2">Check</v-btn>
                    </v-card-title>
                    <v-progress-linear :value="followersMined/user.edge_followed_by.count*100"></v-progress-linear>
                </v-card>
                <v-card class="mb-4 text-center">
                    <v-card-title>
                        #2 Mining Content all followers
                        <v-spacer/>
                        {{ postsMined }}/{{ user.edge_followed_by.count }}
                        <v-btn @click="startMiningPosts" class="ml-2">Tambang</v-btn>
                        <v-btn @click="checkStatusMiningPosts" class="ml-2">Check</v-btn>
                    </v-card-title>
                    <v-progress-linear :value="postsMined/user.edge_followed_by.count*100"></v-progress-linear>
                </v-card>
                <v-btn @click="handelAnalisisFollowers" block>View Data</v-btn>
                <template v-if="followers.mention">
                    <v-card class="mb-4 mt-4">
                        <v-card-title>
                            Most Used Hashtag by followers
                        </v-card-title>
                        <v-card-text>
                            <vue-word-cloud
                                style="
                                    height: 480px;
                                    width: 100%;"
                                :words="followersHashtag"
                                :color="([, weight]) => weight > 10 ? 'DeepPink' : weight > 5 ? 'RoyalBlue' : 'Indigo'"
                                font-family="Roboto"/>
                        
                        </v-card-text>
                    </v-card>
                    <v-card class="mb-4">
                        <v-card-title>
                            Followers Activity
                        </v-card-title>
                        <v-card-text>
                            <calendar-heatmap 
                                :values="[{ date: '2022-8-1', count: 6 }]"
                                :end-date="'2022-8-10'"/>
                            <!-- <vue-chart-heatmap></vue-chart-heatmap> -->
                        
                        </v-card-text>
                    </v-card>
                    <v-card class="mb-4 pr-2">
                        <div>
                            <v-card-title>
                                People Mention By Folowers
                            </v-card-title>
                            <v-list dense class="py-0">
                                <template
                                    v-for="(item, index) in followers.mention.buckets">
                                    <v-list-item
                                        :key="index">
                                        <v-list-item-content>
                                            <v-list-item-title>{{ item.key }}</v-list-item-title>
                                            <v-progress-linear :value="item.doc_count/user.imention.buckets[0].doc_count*100"></v-progress-linear>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                            <v-list-item-action-text>{{ item.doc_count }}</v-list-item-action-text>
                                        </v-list-item-action>
                                    </v-list-item>
                                    <v-divider
                                        :key="`line-${index}`"/>
                                </template>
                            </v-list>
                        </div>
                    </v-card>
                    
                </template>
                <v-card class="mb-4 text-center mt-4">
                    <v-card-title>
                        #3 Analisis Gambar Postingan
                        <v-spacer/>
                        {{ postsMined }}/{{ user.edge_followed_by.count }}
                        <v-btn @click="startMiningPosts" class="ml-2">Tambang</v-btn>
                        <v-btn @click="checkStatusMiningPosts" class="ml-2">Check</v-btn>
                    </v-card-title>
                    <v-progress-linear :value="postsMined/user.edge_followed_by.count*100"></v-progress-linear>
                </v-card>
            </template>
        </v-container>
    </div>
</template>
<script>
import VueWordCloud from 'vuewordcloud';
import { CalendarHeatmap } from 'vue-calendar-heatmap'
export default {
    components: {
        [VueWordCloud.name]: VueWordCloud,
        CalendarHeatmap,
    },
    layout:'apps',
	props: ['apps', 'tipe', 'handelKeluar', 'aesEncrypt', 'aesDecrypt', 'setFetching', 'setSnackbar'],
    computed:{
        interaction: function(){
            let total = 0 
            this.user.edge_owner_to_timeline_media.edges.map((item)=>{
                total += item.node.edge_liked_by.count
                total += item.node.edge_media_to_comment.count
            })
            return total
        },
        score: function(){
            let total       = (((this.interaction/this.user.edge_owner_to_timeline_media.edges.length)/this.user.edge_followed_by.count)*100).toFixed(0)
            return total
        },
        location: function(){
            let data    = this.user.edge_owner_to_timeline_media.edges.filter((item)=>item.node.location!=null).map((item)=>item.node.location)
            let result  = []
            data.map((item)=>{
                if(result.filter((row)=>row.id==item.id).length==0){
                    result.push({id: item.id, name: item.name, total: data.filter((row)=>row.id==item.id).length })
                }
            })

            result.sort((a, b) => {
                return b.total - a.total;
            });
            return result
        },
        tags: function(){
            let data    = this.user.edge_owner_to_timeline_media.edges.map((item)=>item.node.edge_media_to_tagged_user.edges)
            let result  = []
            let temp    = []
            data.map((item)=>{
                item.map((row)=>{
                    temp.push(row.node.user)
                })
            })

            data.map((item)=>{
                item.map((row)=>{
                    if(result.filter((x)=>x.id==row.node.user.id).length==0){
                        result.push({
                            ...row.node.user,
                            total: temp.filter((x)=>x.id==row.node.user.id).length
                        })
                    }
                })
            })
            result.sort((a, b) => {
                return b.total - a.total;
            });
            // console.log(result)
            return result
        },
        mention: function(){
            let data    = this.user.mention.edges.map((item)=>item.node.owner)
            let result  = []
            data.map((item)=>{
                if(result.filter((row)=>row.id==item.id).length==0){
                    result.push({id: item.id, username: item.username, total: data.filter((row)=>row.id==item.id).length })
                }
            })

            result.sort((a, b) => {
                return b.total - a.total;
            });
            return result
        },
        hashtag: function(){
            let result = []

            this.user.hash.buckets.map((item)=>{
                result.push([item.key, item.doc_count])
            })
            // result.push(['romance', 19])
            return result
        },
        followersHashtag: function(){
            let result = []
            this.followers.hash.buckets.map((item)=>{
                result.push([item.key, item.doc_count])
            })
            // result.push(['romance', 19])
            return result
        }
    },
    asyncData: async function({$api, params}){
        const user  = (await $api.$get(`/user/info/${params.username}`)).user
        return {
            user,
            username: params.username
        }
	},
    data: function(){
        return {
            tab: 0,
            followersMined: 0,
            postsMined: 0,
            followers: {}
        }
    },
    mounted: function(){
        this.checkStatusMiningFollowers()
        this.checkStatusMiningPosts()
        // setInterval(this.checkStatusMiningFollowers, 2000)
    },
    methods:{
        startMiningFollowers: function(){
            this.$api.$get(`/mining/followers/${this.username}/${this.user.id}`).then((response)=>{
                console.log(response)
            })
        },
        checkStatusMiningFollowers: async function(){
            this.followersMined = (await this.$api.$get(`/mining/followers-status/${this.username}`)).value
        },
        startMiningPosts: function(){
            this.$api.$get(`/mining/posts/${this.username}/${this.user.id}`).then((response)=>{
                console.log(response)
            })
        },
        checkStatusMiningPosts: async function(){
            this.postsMined = (await this.$api.$get(`/mining/posts-status/${this.username}`)).total
        },
        handelAnalisisFollowers: async function(){
            this.followers = (await this.$api.$get(`/followers/info/${this.username}`))
        }
    }
}
</script>