<template>
    <div>
        <div>
            <span>搜索：</span>
            <input type="text" id="keyword" value="请输入关键字：(选定后搜索)" onfocus='this.value=""'/>
        </div>
        <div id="mapContainer"></div>
        <div>
            <span>定位：</span>
            <input type="text" v-model="selectAddress">
        </div>
        <div>
            <span>经纬度：</span>
            <input type="text" v-model="selectLngLat">
            
        </div>
    </div>
</template>

<script>
export default {
    name: 'search',
    data() {
        return {
           cityLocation: '',
           lnglat: [116.397428, 39.90923],
           zoom: 11,
           selectAddress: '',
           selectLngLat: ''
        }
    },
    mounted() {
        this.searchAddressMethod()
    },
    methods: {

        searchAddressMethod() {
            const self = this

            AMap.plugin(['AMap.CitySearch', 'AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Geocoder', 'AMap.ToolBar'], function () {
                var citySearch = new AMap.CitySearch()
                var map
                // 搜索插件
                var placeSearch

                // 城市定位 begin
                citySearch.getLocalCity(function (status, result) {
                    if (status === 'complete' && result.info === 'OK') {
                        // 查询成功，result即为当前所在城市信息
                        console.log(result)
                        let rectangle = result.rectangle.split(";")[1].split(",")
                        self.lnglat = rectangle.map(Number)

                        // 初始化地图 begin
                        map = new AMap.Map("mapContainer", {
                            resizeEnable: true,
                            // center: self.lnglat,//地图中心点
                            zoom: self.zoom,//地图显示的缩放级别
                            keyboardEnable: false
                        });
                        map.addControl(new AMap.ToolBar());
                        // 初始化地图 end

                        // 绑定点击事件 begin
                        let marker, geocoder
                        AMap.event.addListener(map, "click", function(e) {
                            console.log(e)
                            // 创建标记 begin
                            if(marker != null) {
                                marker.setMap(null)
                            }
                            marker = new AMap.Marker({
                                icon: "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
                                position: e.lnglat,
                                offset: new AMap.Pixel(-13, -30)
                            });
                            marker.setMap(map);
                            // 创建标记 end

                            // 解析位置 begin
                            // 获取经纬度
                            self.selectLngLat = e.lnglat.lat + "，" + e.lnglat.lng
                            if(!geocoder){
                                geocoder = new AMap.Geocoder({
                                    // city: "010", //城市设为北京，默认：“全国”
                                    radius: 1000 //范围，默认：500
                                });
                            }

                            geocoder.getAddress(e.lnglat, function(status, result) {
                                if (status === 'complete'&&result.regeocode) {
                                    console.log(result)
                                    var address = result.regeocode.formattedAddress;
                                    console.log(address)
                                    self.selectAddress = address
                                    
                                }else{
                                    log.error('根据经纬度查询地址失败')
                                }
                            });
                            // 解析位置 end
                        })
                        // 绑定点击事件 end
                    }
                })
                // 城市定位 end

                // 搜索联想提示 begin
                var autoOptions = {
                    input: "keyword"    //使用联想输入的input的id
                };
                var autocomplete= new AMap.Autocomplete(autoOptions);
                AMap.event.addListener(autocomplete, "select", function(e){
                    //TODO 针对选中的poi实现自己的功能
                    console.log(e)
                    placeSearch = new AMap.PlaceSearch({
                        map:map
                    })
                    placeSearch.search(e.poi.name)
                });
                // 搜索联想提示 end
            })
        },

    },
    watch: {
        searchAddress() {

        }
    }
}
</script>

<style>
    
    input {
        width: 300px;
        height: 25px;
        padding-left: 10px;
        margin: 20px 0;
    }

    #mapContainer {
        width: 70vw;
        height: 50vh;
    }

    #mapContainer img {
        width: 20px;
    }
</style>