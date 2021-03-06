﻿CCD測距理論介紹(完整可實現)

資料來源: 全國博碩士論文
GITHUB完整備份: https://github.com/jash-git/CCD-ranging

現有測距方法及其原理
	接觸式測距
	非接觸式測距
		超音波測距
			d 的距離碰到物體反射回來，再由收信器將反射波接收，然後測量其往返的時間ｔ，若空氣中的音速為ｖ
			d=1/2(vt)
		雷射測距		
			飛行時間（Time of flight）量測法
				d=1/2(Ct)
				C：雷射在空氣中傳播的速度（已知值）
				t：雷射在待測距離上往返傳播時間（檢測之量值）				
			相位差量測法
				PDF有對應公式和介紹
			干涉量測法
				PDF有對應公式和介紹
			三角量測法
				PDF有對應公式和介紹

影像式測距
	影像水平寬度之距離演算法
		參01.PFF
	雙CCD 視差法
		參02.PFF
		參02-1.PFF
	散焦測距法
		參03.PFF
	IBDMS 平行量測法
		參04.PFF
		
影像測距是將二維（2-Dimention）的平面影像，利用輔助器材或數學計算，重建目標物的「景深」（Depth）資料。前一章所敘述的各種方法皆是為達到此目的。

因為在道路上行駛的車輛，不論其大小、種類和顏色，其號牌的尺寸都是一樣的（如表1），也就是說，號牌在影像中提供我們一個恆定的參考值，車輛距離愈近，號牌影像愈大，車輛距離愈遠，號牌影像愈小，且車輛距離和號牌尺寸在影像中，應該有一固定的數學關係。當找到此一數學關係後，即可非常容易的利用號牌影像進行測距。