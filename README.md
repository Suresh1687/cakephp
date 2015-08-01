
http://jsfiddle.net/falafelsoftware/yX6kg/
http://www.telerik.com/forums/update-and-create-in-grid-throws-unable-to-get-property-%27data%27-of-undefined-or-null-reference

http://jsfiddle.net/krustev/yqshb/



---------------------------------------------------


    <head>
<link rel="stylesheet" href="styles/kendo.common.min.css" />
<link rel="stylesheet" href="styles/kendo.default.min.css" />

    <script src="js/jquery.min.js"></script>
    <script src="js/kendo.all.min.js"></script>
    </head>
<div id="main">
    <div id="grid" data-bind="source: dataSource"></div>
</div>
    <script>
    (function () {
//var item.countries;

    var multiSelectEditor = function (container, options) {
        $('<input data-bind="value:' + options.field + '"/>')
            .appendTo(container)
            .kendoMultiSelect({
            suggest: true,
            dataSource: options.values
        });
    };
    
    var suggestEditor = function (container, options) {
        $('<input data-bind="value:' + options.field + '"/>')
            .appendTo(container)
            .kendoAutoComplete({
            suggest: true,
            dataSource: options.values
        });
    };
    
    

    var numericEditor = function (container, options) {
        $('<input data-bind="value:' + options.field + '"/>')
            .appendTo(container)
            .kendoNumericTextBox({
            decimals: 2,
            min: 0,
            format: 'c2'
        });
    };

    var multiSelectArrayToString = function (item) {
        return item.countries.join(', ');
    };

 var multiSelectArrayToString1 = function (item) {
        alert(item.product);
        return item.product;
        
    };
    var data=[{
                id: 1,
                product: 'suresh goud shankarampet-502245',
                countries: ['Mexico', 'Canada'],
                price: 0.98
            }, {
                id: 2,
                product: 'Poonam-502300',
                countries: ['United States'],
                price: 2.96
            }, {
                id: 3,
                product: 'Aman kumar-502245',
                countries: [],
                price: 45.90
            }, ];
    var crudServiceBaseUrl = "";
    var vm = kendo.observable({
        countries: ['Canada', 'Mexico', 'United States'],
        suggestdata:['suresh goud shankarampet-502245', 'Poonam-502300', 'Aman kumar-502245', 'Shreyas-502248','X098'],
        dataSource: new kendo.data.DataSource({
//             transport: {
//                                read:  {
////                                    url: crudServiceBaseUrl + "test.php?type=1",
//                                    dataType: "json",
//                                    data: data
//                                },
//                                update: {
//                                    url: crudServiceBaseUrl + "test.php?type=2",
//                                    dataType: "json"
//                                },
//                                destroy: {
//                                    url: crudServiceBaseUrl + "test.php?type=3",
//                                    dataType: "json"
//                                },
//                                create: {
//                                    url: crudServiceBaseUrl + "test.php?type=4",
//                                    dataType: "json"
//                                },
//                                parameterMap: function(options, operation) {
//                                    if (operation !== "read" && options.models) {
//                                        return {models: kendo.stringify(options.models)};
//                                    }
//                                }
//                            },
//                            batch: true,
               data: data,
            schema: {
              
                model: {
                    id: 'id',
                    fields: {
                        'id': {
                            type: 'number'
                        },
                            'product': {
                            type: 'string'
                        },
                            'countries': {},
                            'price': {
                            type: 'number'
                        }
                    }
                }
            }
        })
    });

    $('#grid').kendoGrid({
        columns: [{
            command: 'edit',
            width: '120px'
        }, {
            field: 'product',
            editor: suggestEditor, // function that generates the multiSelect control
            values: vm.suggestdata, // custom property with array of values
            template: multiSelectArrayToString1 // template: how to display text in grid
            
        }, {
            field: 'countries',
            editor: multiSelectEditor, // function that generates the multiSelect control
            values: vm.countries, // custom property with array of values
            template: multiSelectArrayToString // template: how to display text in grid
        }, {
            field: 'price',
            editor: numericEditor,
            format: '{0:c}'
        }],
        editable: 'inline'

    });

    kendo.bind('#main', vm);

})()
</script>

