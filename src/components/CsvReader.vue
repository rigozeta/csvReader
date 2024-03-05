<template>
    <div>
        <strong>Upload a CSV file here</strong>
        <p><input type="file" @change="uploadCsv" /></p>
        <div v-if="csvLoaded">
            <p>
                Select Header: 
                <select v-model="selectedHeaderIdx">
                    <option v-for="(label, idx) in headers" v-bind:value="idx" v-bind:key="idx">{{ label }}</option>
                </select> 
            and
                Row: 
                <input type="number" v-bind:max="headers.length" min="0" v-model="selectedRowIdx"/>
            </p>
            <h3>The value is: {{ getData }}</h3>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CsvReader',
    data: function() {
        return {
            csvFile: '',
            data: null,
            headers: [],
            cols: [],
            csvLoaded: false,
            selectedHeaderIdx: 0,
            selectedRowIdx: 0,
        }
    },
    methods: {
        uploadCsv(e) { // Read through uploaded file
            let _this = this;

            new Promise((resolve) => {
                let file = e.target.files[0];
                let reader = new FileReader();
                reader.readAsText(file);
                reader.onload = (e) => {
                    _this.data = e.target.result;
                    resolve(true)
                }
            }).then(() => {
                _this.parseHeaders();
                return true;
            });
            
            return false;
        },

        parseHeaders() { // Generate headers (1st row) and data content from uploaded file
            let _this = this;
            _this.data = _this.data.replaceAll("\r\n", "\r\n,");
            let explodedData = _this.data.split(",");

            let recordHeader = true;
            let tempCols = [];
            let tempRows = [];

            explodedData.forEach((h) => {
                if(recordHeader) {
                    _this.headers.push(h.replace("\r\n","")) // \r\n - line breaks. Add additional comma
                    if(h.includes("\r\n")) {
                        recordHeader = false;
                    } 
                } else {
                    tempCols.push(h.replace("\r\n","")); // \r\n - line breaks. Add additional comma
                    
                    if(tempCols.length == _this.headers.length) {
                        tempRows.push([...tempCols]);

                        tempCols = [];
                    }

                }
                             
            });

            _this.cols = tempRows;
            _this.csvLoaded = true;
        },

        
    },
    computed: {
        getData: function () { // Display cell data based from selected header (column) and content (row)
            let _this = this;
            let row = _this.cols[_this.selectedRowIdx][_this.selectedHeaderIdx];
            return row;
        },
    }
}
</script>


