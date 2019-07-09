<template>
    <div class="container">
        <div class="row">
            <div class="col-sm-9 col-sm-offset-1">
                <form class="form-inline">
                    <div class="form-group">
                        <label for="inputTitle">标题</label>
                        <input
                            type="text"
                            class="form-control"
                            id="inputTitle"
                            v-model="inputTitle"
                            placeholder="标题"
                        />
                    </div>
                    <div class="form-group">
                        <label for="inputPrice">价格</label>
                        <input
                            type="text"
                            class="form-control"
                            id="inputPrice"
                            v-model="inputPrice"
                            placeholder="价格"
                        />
                    </div>
                    <div class="form-group">
                        <label for="inputDescription">描述</label>
                        <input
                            type="text"
                            class="form-control"
                            id="inputDescription"
                            v-model="inputDescription"
                            placeholder="描述"
                        />
                    </div>
                    <button type="button" class="btn btn-default" @click="onAdd">添加</button>
                </form>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-12 col-sm-offset-1">
                <form class="form-inline">
                    <div class="form-group">
                        <label for="inputSearch">搜索</label>
                        <input
                            type="text"
                            class="form-control"
                            id="inputSearch"
                            v-model="inputSearch"
                            placeholder="输入名称"
                        />
                        <button type="button" class="btn btn-default" @click="onSearch">搜索</button>
                    </div>
                </form>
                <button type="button" class="btn btn-default" @click="onSortUp">价格低到高</button>
                <button type="button" class="btn btn-default" @click="onSortDown">价格高到低</button>
                <button type="button" class="btn btn-default" @click="onSortDefault">默认排序</button>
            </div>
        </div>
        <div class="row">
            <table class="table">
                <thead>
                    <tr>
                        <th>序号</th>
                        <th>名称</th>
                        <th>价格</th>
                        <th>描述</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in dataTable" :key="index">
                        <td>{{index+1}}</td>
                        <td>{{item.title}}</td>
                        <td>{{item.price}}</td>
                        <td>{{item.description}}</td>
                        <button
                            type="button"
                            class="btn btn-primary"
                            data-toggle="modal"
                            data-target="#modalModify"
                            @click="onModalModifyShow(index)"
                        >修改</button>
                        <button type="button" class="btn btn-default" @click="onDelete(index)">删除</button>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="modal fade" id="modalModify" tabindex="-1" role="dialog">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                        <h4 class="modal-title">Modal title</h4>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div class="form-group">
                                <label for="modalTitle">标题</label>
                                <input
                                    type="text"
                                    class="form-control"
                                    id="modalTitle"
                                    v-model="modalTitle"
                                    placeholder="标题"
                                />
                            </div>
                            <div class="form-group">
                                <label for="modalPrice">价格</label>
                                <input
                                    type="text"
                                    class="form-control"
                                    id="modalPrice"
                                    v-model="modalPrice"
                                    placeholder="价格"
                                />
                            </div>
                            <div class="form-group">
                                <label for="modalDescription">描述</label>
                                <input
                                    type="text"
                                    class="form-control"
                                    id="modalDescription"
                                    v-model="modalDescription"
                                    placeholder="描述"
                                />
                            </div>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                        <button
                            type="button"
                            class="btn btn-primary"
                            @click="onModify(modalIndex)"
                            data-dismiss="modal"
                        >修改</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import "@/assets/js/jquery-2.1.1.min.js";
import Header from "@/components/home/header";

export default {
    name: "home",
    components: {
        Header
    },
    data: function() {
        return {
            inputTitle: null,
            inputPrice: null,
            inputDescription: null,
            modalTitle: null,
            modalPrice: null,
            modalDescription: null,
            modalIndex: null,
            inputSearch: null,
            dataList: [
                {
                    title: "西红柿",
                    price: 34,
                    description: "新鲜西红柿"
                },
                {
                    title: "马铃薯",
                    price: 23,
                    description: "新鲜马铃薯"
                },
                {
                    title: "地瓜",
                    price: 13,
                    description: "新鲜地瓜"
                },
                {
                    title: "地瓜",
                    price: 13,
                    description: "新鲜地瓜"
                }
            ],
            dataTable: []
        };
    },
    created() {
        this.initList(this.dataList);
    },
    methods: {
        initList: function(items) {
            return (this.dataTable = JSON.parse(JSON.stringify(items)));
        },
        onAdd: function() {
            var item = {
                title: this.inputTitle,
                price: this.inputPrice,
                description: this.inputDescription
            };
            this.dataList.push(item);
            this.inputTitle = "";
            this.inputPrice = "";
            this.inputDescription = "";
            this.initList(this.dataList);
        },
        onDelete: function(index) {
            this.dataList.splice(index, 1);
            this.initList(this.dataList);
        },
        onModalModifyShow: function(index) {
            this.modalTitle = this.dataList[index].title;
            this.modalPrice = this.dataList[index].price;
            this.modalDescription = this.dataList[index].description;
            this.modalIndex = index;
        },
        onModify: function(modalIndex) {
            var dataModify = Object.assign({}, this.dataList[modalIndex], {
                title: this.modalTitle,
                price: this.modalPrice,
                description: this.modalDescription
            });
            this.$set(this.dataList, modalIndex, dataModify);
            this.initList(this.dataList);
        },
        onSearch: function() {
            if (this.inputSearch) {
                var textSearch = this.inputSearch.toString();
                var dataSearch = [];
                for (var i = 0; i < this.dataList.length; i++) {
                    if (this.dataList[i].title.indexOf(textSearch) > -1) {
                        dataSearch.push(this.dataList[i]);
                    }
                }
                this.initList(dataSearch);
            } else {
                this.initList(this.dataList);
            }
        },
        onSortDefault: function() {
            this.initList(this.dataList);
        },
        onSortUp: function() {
            this.dataTable.sort(function(a, b) {
                return a.price - b.price;
            });
        },
        onSortDown: function() {
            this.dataTable.sort(function(a, b) {
                return b.price - a.price;
            });
        }
    },
    watch: {}
};

$(document).ready(function() {});
</script>

<style lang="scss">
.home-container {
    display: flex;
    flex-direction: column;
}
</style>


