<template>
    <div class="container">
        <div class="revenue-table">
             <!-- Button trigger modal -->
            <button class="btn add-revenue-btn" data-toggle="modal" data-target="#addRevenueModal">
                Add Revenue Group
            </button>
            <table>
                <tr>
                    <th>Revenue Group Title</th>
                    <th>Revenue Percentage</th>
                    <th>Action</th>
                </tr>
                <tr v-for="(revenue, index) in revenues" :key="index">
                    <td>{{ revenue.title }}</td>
                    <td>{{ revenue.revenue_percentage}}</td>
                    <td><button class="btn delete-revenue-btn" @click="deleteRevenue(index)">Delete</button></td>
                </tr>
            </table>
            <!-- Modal -->
            <div class="modal fade" id="addRevenueModal" tabindex="-1" role="dialog" aria-labelledby="addRevenueModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h4 class="modal-title" id="addRevenueModalLabel">Add Revenue Group</h4>
                        </div>
                        <div class="modal-body">
                            <h5 class="revenue-group-title">Revenue Group Title</h5>
                            <input v-model="revenues_items.title" type="text" id="revenue-group-textbox" class="revenue-group-textbox"/><br>
                            <br>
                            <div class="condition-dropdown">
                                If
                                <select v-model="revenues_items.conditions" name="conditions" id="conditions">
                                    <option value="all">ALL</option>
                                </select>
                                of the below conditions are met
                            </div>
                            
                            <div class="subcontainer">
                                <h5 class="rule-title" v-if="isAddingRule == true">Rule 1</h5>
                                <div class="default-rule">
                                    <select v-model="revenues_items.rule_dropdown_one" name="rule-dropdown-one" id="rule-dropdown-one">
                                        <option value="aff_sub">aff_sub</option>
                                    </select>
                                    <select v-model="revenues_items.rule_dropdown_two" name="rule-dropdown-two" id="rule-dropdown-two">
                                        <option value="is">is</option>
                                    </select>
                                    <input v-model="revenues_items.parameter_one" type="text" id="rule-textbox" class="rule-textbox" placeholder="insert parameter"/>
                                    <span v-if="isNewRuleOne == false">
                                        <a href="javascript:void(0)" class="addRule" id="addRule" @click="addParameterOne()">add rule</a>
                                    </span>
                                    <span v-else>
                                        <a href="javascript:void(0)" class="removeRule" id="removeRule" @click="removeParameterOne()">remove rule</a>
                                    </span>
                                    <span v-if="isNewRuleOne == false || isAddingRule == false">
                                        <a href="javascript:void(0)" class="btnMinus" id="btnMinus" @click="removeRule()"><i class="fas fa-minus-circle"></i></a>
                                    </span>
                                    <span v-if="isAddingRule == false">
                                        <a href="javascript:void(0)" class="btnAdd" id="btnAdd" @click="addMoreRule()"><i class="fas fa-plus-circle"></i></a>
                                    </span>
                                </div>
                                <div class="added-rule" v-if="isNewRuleOne == true">
                                    <input v-model="revenues_items.parameter_two" type="text" id="rule-textbox-two" class="rule-textbox-two" placeholder="insert parameter"/>
                                    <a v-if="isNewRuleOne == true" href="javascript:void(0)" class="addRule" id="addRule">add rule</a>
                                    <a v-else href="javascript:void(0)" class="removeRule" id="removeRule">remove rule</a>
                                    <span>
                                        <a href="javascript:void(0)" class="btnMinus" id="btnMinus" @click="removeRule()"><i class="fas fa-minus-circle"></i></a>
                                    </span>
                                    <span v-if="isAddingRule == false">
                                        <a href="javascript:void(0)" class="btnAdd" id="btnAdd" @click="addMoreRule()"><i class="fas fa-plus-circle"></i></a>
                                    </span>
                                </div>
                            </div>
                            <div class="subcontainer-two" v-if="isAddingRule == true">
                                <h5 class="rule-title" v-if="isAddingRule == true">Rule 2</h5>
                                <div class="default-rule">
                                    <select v-model="revenues_items.rule_dropdown_three" name="rule-dropdown-one" id="rule-dropdown-one">
                                        <option value="aff_sub">aff_sub</option>
                                    </select>
                                    <select v-model="revenues_items.rule_dropdown_four" name="rule-dropdown-two" id="rule-dropdown-two">
                                        <option value="is">is</option>
                                    </select>
                                    <input v-model="revenues_items.added_parameter_one" type="text" id="rule-textbox" class="rule-textbox" placeholder="insert parameter"/>
                                    <span v-if="isNewRuleTwo == false">
                                        <a href="javascript:void(0)" class="addRule" id="addRule" @click="addParameterTwo()">add rule</a>
                                    </span>
                                    <span v-else>
                                        <a href="javascript:void(0)" class="removeRule" id="removeRule" @click="removeParameterTwo()">remove rule</a>
                                    </span>
                                    <a href="javascript:void(0)" class="btnMinus" id="btnMinus" @click="removeRule()"><i class="fas fa-minus-circle"></i></a>
                                    <a href="javascript:void(0)" class="btnAdd" id="btnAdd" @click="addMoreRule()"><i class="fas fa-plus-circle"></i></a>
                                </div>
                                <div class="added-rule" v-if="isNewRuleTwo == true">
                                    <input v-model="revenues_items.added_parameter_two" type="text" id="rule-textbox-two" class="rule-textbox-two" placeholder="insert parameter"/>
                                    <a v-if="isNewRuleTwo == true" href="javascript:void(0)" class="addRule" id="addRule">add rule</a>
                                    <a v-else href="javascript:void(0)" class="removeRule" id="removeRule">remove rule</a>
                                    <a href="javascript:void(0)" class="btnMinus" id="btnMinus"><i class="fas fa-minus-circle"></i></a>
                                    <a href="javascript:void(0)" class="btnAdd" id="btnAdd"><i class="fas fa-plus-circle"></i></a>
                                </div>
                            </div>
                            <div class="revenue-percentage">
                                then revenue is <input v-model="revenues_items.revenue_percentage" type="number" id="revenue-percent" class="revenue-percent"/><i class="fas fa-percent"></i>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btnConfirm" @click="addRevenue()">Confirm</button>
                            <button type="button" class="btn btnCancel" data-dismiss="modal">Cancel</button>
                        </div>
                    </div><!-- /.modal-content -->
                </div><!-- /.modal-dialog -->
            </div><!-- /.modal -->
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isAddingRule: false,
            isNewRuleOne: false,
            isNewRuleTwo: false,
            revenues: [],
            revenues_items: {
                title: '',
                conditions: 'all',
                rule_dropdown_one: 'aff_sub',
                rule_dropdown_two: 'is',
                rule_dropdown_three: 'aff_sub',
                rule_dropdown_four: 'is',
                parameter_one: '',
                added_parameter_one: '',
                parameter_two: '',
                added_parameter_two: '',
                revenue_percentage: '',
            },
        }
    },
    methods: {
        addMoreRule() {
            this.isAddingRule = true;
        },
        removeRule() {
            this.isAddingRule = false;
        },
        addParameterOne() {
            this.isNewRuleOne = true;
        },
        removeParameterOne() {
            this.isNewRuleOne = false;
        },
        addParameterTwo() {
            this.isNewRuleTwo = true;
        },
        removeParameterTwo() {
            this.isNewRuleTwo = false;
        },
        addRevenue() {
            this.revenues.push({...this.revenues_items});

            if (this.isAddingRule == false) {
                this.revenues_items.rule_dropdown_three = '';
                this.revenues_items.rule_dropdown_four = '';
                this.revenues_items.added_parameter_one = '';
                this.revenues_items.added_parameter_two = '';
            } else if (this.isNewRuleOne == false) {
                this.revenues_items.parameter_two = '';
            } else if (this.isNewRuleTwo == false) {
                this.revenues_items.added_parameter_two = '';
            }
        },
        deleteRevenue(index) {
            this.revenues.splice(index, 1);
        }
    }
}
</script>