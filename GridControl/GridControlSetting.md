# 1. GridControl
1. �÷� Ŭ�� ����
2. ���� �ٲ�� �ٸ������� �������ִ� ���
	- ������ �ٲ� ���̺��� ��ť�ɾ �����ο쿡 �ִ� �ٸ� �������� �ҷ��� �� �ִ�. 
3. ���� ũ��� �÷����(�ҵ��� ��� ���) ũ�� ���� ���� �ٲ��� �� �ִ�.
4. �ο켿�鿡�� ���� �ֱ�(readonly ȸ�� ����)
5. �׷��� �� �÷�����
	- �÷� ���� �̺�Ʈ���� (����ǥ����)
	- ǲ�Ϳ� ��, ��� �� ��갪 �������ֱ�
		- ǲ��(footer)�� ����
6. �׸������ ���� : ���׸���� (BandedGridView) 
	- BandedGridView �÷� Header �� �����ֱ�
7. �����÷�
	- �����÷� CU �����Ҷ� �÷� �߶� �ѱ��. �Ʒ� �� �޼���� �ٷ� �� DataSave() ���� ����� ���ߴµ� ����Ҽ��� ������ ����ٶ�. ��ó�� MS Q&A
8. �׸������ �ο츦 �� �Ʒ��� �Ű��ֱ�(���Ʒ� ��ư�����)
9. �ο� �߰��Ҷ� ������� ��ȣ�� ���ڵ� �ֱ�
10. ���Խ����� �÷��� ���������� �ۼ��� �� �ְ� ������ֱ�
11. �׸���� ���Ϳ� ���ؼ� 
	- ���͸� �Ⱥ��̰� �ϴ¹�
	- ���Ͱ� BOOLEAN �϶� �⺻ ����Ʈ���� FALSE�� �ִ� ��� (�⺻�� NULL�ε� �ϴ�)
12. �׸��� ��Ʈ�� ȭ��ܿ� ���ʿ��� 0 �� ������ �Ⱥ��̰� �ϴ� ���
_________________________________________________________________________
#### 1. �÷� Ŭ�� ����

�Ʒ��� �׸������ �÷��� ����ڰ� �������� ���ϰ� ���� �̺�Ʈ�� ����ߴ�.
ShowingEditor�� ����ϰ� e.cancle = true; �� �ϸ� ������ ���� �ʴ´�. 
����ġ���� ����ؼ� Ư�� �÷����� ������� ������ ���� �� �ִ�.
gridview.FocusedColumn.FieldName ���� ������(��Ŀ����) ���� �÷��̸��� ���Ͽ�
���ϴ� �÷����� showingeditor �̺�Ʈ�� ����� �� �ִ�. 
```c#
Grid_Something.ShowingEditor += Grid_Something_ShowingEditor;

private void Grid_Something_ShowingEditor(object sender, System.ComponentModel.CancelEventArgs e)
       {
           switch (Grid_Something.DefaultView.FocusedColumn.FieldName)
           {
               case "CAR_NUMBER":
               case "PHONE_NUMBER":
               case "TON_CAR":
               case "FC_NAME":
                   e.Cancel = true;
                   break;
               default:
                   e.Cancel = false;
                   break;
           }
          
       }
```
_________________________________________________________________________
#### 2. ���� �ٲ�� �ٸ������� �������ִ� ���
CellValueChanged �̺�Ʈ�� ����ؼ� ������ ���� �ٲ� �Ϸ������� ������ �� �ִ�.
``` C#
gridview.CellValueChanged += gridview_CellValueChanged;
```
���� ������ ������ �ٲ� �ٸ������� ���� �־��� �� �ְԵȴ�. ������ LookUp���� ������
�����ϸ� ���� �ο��� �ٸ� �������� �ڵ����� ���ε��Ǵ� ���� ���� ����.
``` C#
private void gridview_CellValueChanged(object sender, DevExpress.XtraGrid.Views.Base.CellValueChangedEventArgs e)
      {
          var item = this.gridview.GetFocusedRow<ISOMETHINGModel>();
          if (item == null) return;

          //SOMETHING_ID������ �ٲ��ָ� ������ȣ,����ó,�������,�ŷ�ó �� �ڵ����� �ٿ��ݴϴ�. 
          if(e.Column.FieldName == "SOMETHING_ID")
          {
              var items = this.SomethingDataTable.AsEnumerable().Where(i => i["SOMETHING_ID"].ToString() == item.SOMETHING_ID.ToString());

              if(items.Count() > 0)
              {
                  DataRow row = items.FirstOrDefault();
                  if (row["CAR_NUMBER"] != DBNull.Value)
                  {
                      item.CAR_NUMBER = row["CAR_NUMBER"].ToString();
                  }
                  if (row["PHONE_NUMBER"] != DBNull.Value)
                  {
                      item.PHONE_NUMBER = row["PHONE_NUMBER"].ToString();
                  }
                  if (row["TON_CAR"] != DBNull.Value)
                  {
                      item.TON_CAR = row["TON_CAR"].ToString();
                  }
                  if (row["FC_NAME"] != DBNull.Value)
                  {
                      item.FC_NAME = row["FC_NAME"].ToString();
                  }
              }
          }
```
���� �ڵ带 ���� ������ �׸������ �ο츦 GetFocusedRow\<T>() �޼��带 ����ؼ� item�� �ٿ��־���. 
```C#
if(e.Column.FieldName == "SOMETHING_ID")
```
���� if������ �Ķ���� e�� �ٲ� ������ �÷��� �����Ѵ�. 
________________________________________________________________________________________________
#### 3. ���� ũ��� �÷����(������ ��� ���) ũ�� ���� ���� �ٲ��� �� �ִ�.
����׸������ ����� �׳� �׸������ ����� �ٸ��ϱ� �����ؾ��Ѵ�. UI�����ǿ����� ���������ϴ�. 
�׷��� ��Ʈ�ѿ��� �����ϴ°��� ����. �ȱ׷��� ��� ��� ������ �ٲپ����� �� �𸣰� �ȴ�.
- XtraGrid BandedGridView.BandPanelRowHeight Property.
- XtraGrid GridView.ColumnPanelRowHeight Property.

UI������ ����� ���� ������Ƽ�� ������ �� �� �ֱ� �ϴ�.
�̸����� �˾ƿ��µ� �����ϰ� �� �� �ִ�.
�Ʒ� �ڵ�� �̺�Ʈ ���̴�. ���� ���� ����� ����������. �߿��Ѱ��� ��Ʈ ũ��� ���� ����
ũ��� ���� ����� ���ٴ� ���̴�.!! ��Ʈũ�⸦ Ű���ָ� �ڵ����� ���� ũ�⵵ �þ�� �� �� ������
�� �׷������� �ʴ�. ���� ũ�⸦ �������ִ� ����� 

- �̺�Ʈ�� �ϳ��ϳ� �����ɶ� �÷��ִ� ���(Calc.RowHeight())
- UI���� �ʱ� ���ð��� �ٲ��ִ� ���

�� �ִ�.
```C#
//SetGridControl
        
//CustomDrawCell �̺�Ʈ ������. �� ���� �ȵ�. RowCellStyle ���.
//((BandedGridView)this.gridview).CustomDrawCell += SomethingController_CustomDrawCell;
((BandedGridView)this.gridview).RowCellStyle += SomethingController_RowCellStyle;
//�ʿ�� CalcRowHeight�̺�Ʈ�� �� ũ�⺯�� ����.
((BandedGridView)this.gridview).CalcRowHeight += SomethingController_CalcRowHeight;
((BandedGridView)this.gridview).CustomDrawBandHeader += SomethingController_CustomDrawBandHeader;
((BandedGridView)this.gridviewv).BandPanelRowHeight = 40;
```
```C#
//bandinfo�� readonly�� ���� ����Ʈ�� �ٲ��ݴϴ�. 
private void SomethingController_RowCellStyle(object sender, DevExpress.XtraGrid.Views.Grid.RowCellStyleEventArgs e)
{
    if (e.Column.OptionsColumn.ReadOnly)
        e.Appearance.BackColor = Color.Empty;
    //e.Appearance.Font = new Font("", 15);
    e.Appearance.FontSizeDelta = 10;
}
//���� ũ�⸦ �������ݴϴ�. 
private void SomethingController_CalcRowHeight(object sender, DevExpress.XtraGrid.Views.Grid.RowHeightEventArgs e)
{
    e.RowHeight = 60; 
}
//�������� ũ�⸦ �����մϴ�.
private void SomethingController_CustomDrawBandHeader(object sender, BandHeaderCustomDrawEventArgs e)
{
    e.Appearance.FontSizeDelta = 10;            
}
```

_____________________________________________________________________________________