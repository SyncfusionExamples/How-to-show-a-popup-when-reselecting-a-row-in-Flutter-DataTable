# How to show a popup when reselecting a row in Flutter DataTable? 

In this article, we will show you how to show a popup when reselecting a row in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) with the necessary properties. Use a [DataGridController](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridController-class.html) to track selected rows via [DataGridController.selectedRows](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridController/selectedRows.html). Handle the [onCellTap](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/onCellTap.html) callback to detect when a cell is tapped. If a row is already selected, extract its cell values and display them in a popup (AlertDialog).

```dart
SfDataGrid(
        source: employeeDataSource,
        selectionMode: SelectionMode.single,
        navigationMode: GridNavigationMode.cell,
        columnWidthMode: ColumnWidthMode.fill,
        controller: dataGridController,
        onCellTap: (details) {
          final rowIndex = details.rowColumnIndex.rowIndex;
          if (rowIndex > 0) {
            final selectedRow = employeeDataSource.effectiveRows[rowIndex - 1];

            if (dataGridController.selectedRows.contains(selectedRow)) {
              final cellValues = selectedRow
                  .getCells()
                  .map((cell) => cell.value.toString())
                  .toList();
              final labels = [
                'Employee ID',
                'Employee Name',
                'Designation',
                'Salary'
              ];

              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  shape: const RoundedRectangleBorder(
                    borderRadius: BorderRadius.all(Radius.circular(32.0)),
                  ),
                  content: SizedBox(
                    height: 300,
                    width: 300,
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        ...List.generate(labels.length, (index) {
                          return Row(
                            children: [
                              Text(labels[index]),
                              const SizedBox(width: 10),
                              const Text(':'),
                              const SizedBox(width: 10),
                              Text(cellValues[index]),
                            ],
                          );
                        }),
                        SizedBox(
                          width: 300,
                          child: ElevatedButton(
                            onPressed: () => Navigator.pop(context),
                            child: const Text("OK"),
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
              );
            }
          }
        },
        columns: <GridColumn>[
          GridColumn(
              columnName: 'id',
              label: Container(
                  padding: EdgeInsets.all(16.0),
                  alignment: Alignment.center,
                  child: Text(
                    'ID',
                  ))),
          GridColumn(
              columnName: 'name',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text('Name'))),
          GridColumn(
              columnName: 'designation',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text(
                    'Designation',
                    overflow: TextOverflow.ellipsis,
                  ))),
          GridColumn(
              columnName: 'salary',
              label: Container(
                  padding: EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: Text('Salary'))),
    ],
),
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-show-a-popup-when-reselecting-a-row-in-Flutter-DataTable).