<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app clipped flating temporary>
     <v-list dense>
       <v-row class="ma-2 mb-3">
       <v-icon class="mx-4" large>mdi-video</v-icon>
       <v-toolbar-title class="align-center">
           <span class="title">Bitci Youtube</span>
       </v-toolbar-title>   
       </v-row>
       <v-list-item v-for="item in items" :key="item.icon" link> 
         <v-list-item-action>
           <v-icon>{{item.icon}}</v-icon>
           </v-list-item-action>
           <v-list-item-content>
           <v-list-icon-title>
             {{item.text}}
           </v-list-icon-title>
           </v-list-item-content>
         
       </v-list-item>
     </v-list>
    </v-navigation-drawer>
    <v-app-bar app clipped-left>
      <v-app-bar-nav-icon @click="drawer=!drawer"/>
       <v-icon class="mx-4" large>mdi-video</v-icon>
       <v-toolbar-title class="align-center">
           <span class="title">Bitci Youtube</span>
       </v-toolbar-title>   
       <v-spacer/>
       <v-text-field class="mt-6" solo placeholder="Ara.." single-line append-icon="mdi-magnify" color="white"/>
       <v-spacer/>
       <v-btn class="ml-0" icon>
         <v-icon>mdi-apps</v-icon>
       </v-btn>
       <v-btn class="ma-2" icon>
         <v-icon>mdi-bell</v-icon>
       </v-btn>
       <div>
         <v-menu v-model="menu" :close-on-content-click="false" :nudge-width="250" offset-x>
           <template v-slot:activator="{on}">
             <v-avatar size="40">
                <img v-on="on" src="data:image/png;base64,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" />
             </v-avatar>
           </template>  

           <v-card>
             <v-list>
               <v-list-item>
                  <v-list-item-avatar> </v-list-item-avatar>
                  <v-list-item-content> 
                    <v-list-item-title>Cansu Demirhan</v-list-item-title>
                    <v-list-item-subtitle>Bitci.com</v-list-item-subtitle>
                  </v-list-item-content>
               </v-list-item>
             </v-list>
             <v-divider/>
             <v-list>
               <v-list-item>
                 <v-list-item-action>
                   <v-icon>mdi-cog</v-icon>
                 </v-list-item-action>
                <v-list-item-title>Ayarlar</v-list-item-title>
               </v-list-item>
             </v-list>
                <v-list>
               <v-list-item>
                 <v-list-item-action>
                   <v-switch v-model="theme" color="orange"/>
                 </v-list-item-action>
                 <v-list-item-title>Karanlık Tema</v-list-item-title>
               </v-list-item>
             </v-list>
             <v-card-actions>
               <v-spacer/>
               <v-btn color="primary" text @click="menu=false" >
                 Çıkış Yap
                 </v-btn>
             </v-card-actions>
           </v-card>

        </v-menu>
       </div>
    </v-app-bar>

    <v-content>
      <v-container>
        <v-row class="mb-4">
          <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRlQFM2aEhl9SEe3YLBDJnpI3BqiqcQojT6ug&usqp=CAU`" 
               :lazy-src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRlQFM2aEhl9SEe3YLBDJnpI3BqiqcQojT6ug&usqp=CAU`"/>
                <v-card-subtitle class=""> Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>KSK Token</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            
          </v-col>
           
            <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR5lrTtwidiM05AY7rnEl9cLZCJOzpiT_5TQA&usqp=CAU`" />
                <v-card-subtitle class=""> Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>Kripto Sohbet</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>
            
             <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRsClTzKG7KkBD3-_VtLYFB6aXKoOKIeEAPvw&usqp=CAU`" />
                <v-card-subtitle class=""> Bitci.com</v-card-subtitle>
                <v-card-text class="text--primary"><div>ANKA Token Arz</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>

             <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://cdn.sporx.com/img/59/2020/20201027_2_45043663_59229567.jpg`" 
               :lazy-src="`https://cdn.sporx.com/img/59/2020/20201027_2_45043663_59229567.jpg`"/>
                <v-card-subtitle class=""> Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>Karşıyaka Spor Kulübü ile anlaşma imzalandı</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>
             <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://i.ytimg.com/vi/sXDJOBsyaxs/mqdefault.jpg`" 
               :lazy-src="`https://i.ytimg.com/vi/sXDJOBsyaxs/mqdefault.jpg`"/>
                <v-card-subtitle class="">Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>Kripto Para Analizi</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>
             <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcROAZGpBNLtmd6dNYZsMCeiXmTWH-BYzLdCDw&usqp=CAU`" 
               :lazy-src="`https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcROAZGpBNLtmd6dNYZsMCeiXmTWH-BYzLdCDw&usqp=CAU`"/>
                <v-card-subtitle class="">Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>McLaren ile anlaşma</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>
             <v-col sm="6" md="4" lg="3" xl="2" >
             <v-card>
               <v-img height="200px" :src="`https://i.ytimg.com/vi/yQLKpNnlwHQ/maxresdefault.jpg`" 
               :lazy-src="`https://i.ytimg.com/vi/yQLKpNnlwHQ/maxresdefault.jpg`"/>
                <v-card-subtitle class=""> Bitci.com </v-card-subtitle>
                <v-card-text class="text--primary"><div>Bitci kazandırmaya devam ediyor</div></v-card-text>
                <v-card-actions>
                  <v-btn color="orange" text>Kaydet</v-btn>
                  <v-btn color="orange" text>Paylaş</v-btn>
                </v-card-actions>
             </v-card>
            </v-col>
        </v-row>
      </v-container>
      
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    items:[
      {icon:"mdi-trending-up",text:"Popüler İçerik"},
      {icon:"mdi-youtube-subscription",text:"Takip"},
      {icon:"mdi-history",text:"Geçmiş"},
      {icon:"mdi-playlist-play",text:"Oynatma Listesi"},
      {icon:"mdi-clock",text:"Saat"},
    ],
    drawer:false,
    menu:false,
    theme:true
  }),
  watch:{
    theme:function(next){
      this.$vuetify.theme.dark=next
    }
  }
};
</script>
