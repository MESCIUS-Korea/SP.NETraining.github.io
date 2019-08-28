---
title: Spread.NET 필터 & 그룹핑
tags: [필터, 그룹핑, filter, grouping]
keywords: spread.net 필터, 스프레드 닷넷, spread.net 그룹핑
last_updated: Aug 08, 2019
summary: "Spread.NET 필터 & 그룹핑"
sidebar: mydoc_sidebar
permalink: spwin_filter.html
folder: mydoc
---

## 고정 행을 정렬/필터에서 제외하기

사람들은 종종 행을 고정하여 총합 정보를 표시하고, 필터 및 정렬로 정보를 선택해서 조회합니다. 실제 예시를 통해 고정 행을 필터링과 정렬에 포함되지 않도록 설정하는 법을 소개하겠습니다.

1.  Spread 초기화  
    
    ```
    private void InitSpread()
    {
               // 행열 수 설정
    	this.fpSpread1.Sheets[0].RowCount = 10;
    	this.fpSpread1.Sheets[0].ColumnCount = 10;
    
                //정열, 필터링 가능 열 설정
    	this.fpSpread1.Sheets[0].Columns[0].AllowAutoSort = true;
    	this.fpSpread1.Sheets[0].Columns[1].AllowAutoFilter = true;
    
    
                //고정행 설정
    	this.fpSpread1.Sheets[0].FrozenTrailingRowCount = 2;
    	this.fpSpread1.Sheets[0].FrozenRowCount = 1;
    
    }
    코드 복사
    ```
    
2.  UnfilteredRows로 필터링 불가 행 설정  
    
    ```
    private void SetUnfilterRow()
    {
                int[] unfilterRows=new int[3]{0,8,9};
    
                this.fpSpread1.Sheets[0].RowFilter.UnfilteredRows = unfilterRows;
            }
    코드 복사
    ```
    
3.  SortRows로 정렬 불가 행 설정  
    
    ```
    bool ascending = true;
    
    private void fpSpread1_AutoSortingColumn(object sender, FarPoint.Win.Spread.AutoSortingColumnEventArgs e)
    {
    
                e.Cancel = true;
                ascending = !ascending;
                //정렬 가능 행 설정
    	this.fpSpread1.Sheets[0].SortRows(1, 4, new FarPoint.Win.Spread.SortInfo[] { new 
    	FarPoint.Win.Spread.SortInfo(e.Column, ascending) });
    
    }
    
    
    코드 복사
    ```
    

작업 전 이미지:

![](https://www.grapecity.co.kr/images/training/spread/tc_winforms5-1-1.png)

  

정렬 후 이미지:

![](https://www.grapecity.co.kr/images/training/spread/tc_winforms5-1-2.png)

  

필터 후 이미지:

![](https://www.grapecity.co.kr/images/training/spread/tc_winforms5-1-3.png)

  

간단한 샘플을 참고해 주시기 바랍니다.

[샘플 다운로드](https://www.grapecity.co.kr/files/SpreadNET/Samples/WinformsSample/filter_rowfilter.zip)

## [WinForms] 그룹화 라인을 설정하는 방법

지정된 열에 동일한 값을 구현하는 방법 행은 그룹으로 나뉩니다.

GroupDataModel과 같은 관련 속성을 사용하여 그룹화를 설정할 수 있습니다.

```
1.	
        ///
2.	        ///
3.	private void CreateGroupFooter() 
4.	        {
5.	             // 기본 그룹화 열 추가
6.	            SheetView sheet = fpSpread1.ActiveSheet;
7.	            sheet.AllowGroup = true;
8.	            GroupDataModel gdm = newGroupDataModel(sheet.Models.Data);
9.	            sheet.Models.Data = gdm;
10.	            //SortInfo 생성자 SortInfo(0, true) 첫 번째 인수는 그룹화 열 인덱스에 사용됩니다 。
11.	            SortInfo[] siList = newSortInfo[] { newSortInfo(0, true) };
12.	            gdm.Group(siList, null);
13.	
14.	            // 기본 그룹 설정
15.	            DefaultGroupFooter dgf = fpSpread1.Sheets[0].DefaultGroupFooter[0];
16.	            ISheetDataModel dataModel = dgf.DataModel;
17.	
18.	            FarPoint.Win.Spread.CellType.TextCellType a = newTextCellType();
19.	            this.fpSpread1_Sheet1.Columns.Get(0).CellType = a;
20.	            // Footer에 수식을 설정          
21.	  (dataModel asIAggregationSupport).SetCellAggregationType(0, 3, AggregationType.Sum);
22.	            (dataModel asIAggregationSupport).SetCellAggregationFormat(0, 3, "数量合计:{0}");
23.	
24.	            this.fpSpread1.ActiveSheet.GroupFooterVisible = true;
25.	
26.	            //열 너비 가져오기
27.	            this.fpSpread1.ActiveSheet.Columns[3].Width = this.fpSpread1.ActiveSheet.Columns[3].GetPreferredWidth();
28.	        }
```

간단한 샘플을 참고해 주시기 바랍니다.

  

[샘플 다운로드](https://www.grapecity.co.kr/files/SpreadNET/Samples/WinformsSample/spread_win_groupfooter.zip)