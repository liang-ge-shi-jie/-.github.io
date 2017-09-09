<!DOCTYPE html>  
<html>  
<head>  
<meta charset="utf-8"/>  
<title>异步加载</title>  
<script type="text/javascript">  
function initialize() {  
  var mp = new BMap.Map('map');  
  mp.centerAndZoom(new BMap.Point(116.404, 39.915),11);
  mp.enableScrollWheelZoom(true);
  var opts = {type: BMAP_NAVIGATION_CONTROL_SMALL}    
  mp.addControl(new BMap.NavigationControl(opts));

  //mp.addControl(new BMap.NavigationControl()); 
  var opts = {offset: new BMap.Size(150, 5)}    
  mp.addControl(new BMap.ScaleControl(opts));
  //mp.addControl(new BMap.ScaleControl());    
  mp.addControl(new BMap.OverviewMapControl());    
  mp.addControl(new BMap.MapTypeControl()); 
  mp.addControl(new BMap.CeolocationControl());
  mp.addControl(new BMap.CopyrightControl());
  
    var panorama = new BMap.PanoramaControl();  
	panorama.setOffset(new BMap.Size(20, 20));  
	mp.addControl(panorama);
	panorama.setPanoramaPOIType(BMAP_PANORAMA_POI_CATERING); //餐饮 
    panorama.setPov({pitch: 6, heading: 138});
    
    
    
    
    
		    var myStyleJson=[  
		{  
		    "featureType": "road",  
		    "elementType": "geometry.stroke",  
		    "stylers": {  
		        "color": "#ff0000"  
		    }  
		}];
		mp.setMapStyle({styleJson: myStyleJson });
  
   var transit = new BMap.TransitRoute("北京市");    
		transit.setSearchCompleteCallback(function(results){    
		 if (transit.getStatus() == BMAP_STATUS_SUCCESS){    
		   var firstPlan = results.getPlan(0);    
		   // 绘制步行线路    
		   for (var i = 0; i < firstPlan.getNumRoutes(); i ++){    
		     var walk = firstPlan.getRoute(i);    
		     if (walk.getDistance(false) > 0){    
		       // 步行线路有可能为0  
		       mp.addOverlay(new BMap.Polyline(walk.getPoints(), {lineColor: "green"}));    
		     }    
		   }    
		  // 绘制公交线路   
		   for (i = 0; i < firstPlan.getNumLines(); i ++){    
		     var line = firstPlan.getLine(i);    
		     mp.addOverlay(new BMap.Polyline(line.getPoints()));    
		   }    
		   // 输出方案信息  
		   var s = [];    
		   for (i = 0; i < results.getNumPlans(); i ++){    
		     s.push((i + 1) + ". " + results.getPlan(i).getDescription());    
		   }    
		   document.getElementById("log").innerHTML = s.join("<br>");    
		 }    
		})    
		transit.search("中关村", "国贸桥");
  
  var opts = {    
 width : 250,     // 信息窗口宽度    
 height: 100,     // 信息窗口高度    
 title : "Hello"  // 信息窗口标题   
}    
var infoWindow = new BMap.InfoWindow("World", opts);  // 创建信息窗口对象    
mp.openInfoWindow(infoWindow, map.getCenter());      // 打开信息窗口

var polyline = new BMap.Polyline([    
   new BMap.Point(116.399, 39.910),    
   new BMap.Point(116.405, 39.920)    
 ],    
 {strokeColor:"blue", strokeWeight:6, strokeOpacity:0.5}    
);    
mp.addOverlay(polyline);
}  
   
function loadScript() {  
  var script = document.createElement("script");  
  script.src = "http://api.map.baidu.com/api?v=2.0&ak=WSVtLpLBweFzWiLbXbGCUyn6IxYcRXfR&callback=initialize";//此为v2.0版本的引用方式  
  // http://api.map.baidu.com/api?v=1.4&ak=您的密钥&callback=initialize"; //此为v1.4版本及以前版本的引用方式  
  document.body.appendChild(script);  
}  
   
window.onload = loadScript;  
</script>  
</head>  
<body>  
  <div id="map" style="width:800px;height:520px"></div>  
</body>  
</html>
