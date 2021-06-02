<template>
    <div class="w-full max-w-lg grid gap-6">
        <form @submit.prevent="handleSubmit" class="bg-gray-100 flex flex-col space-y-4 p-6 rounded-md">
            <label for="amount" class="flex flex-col">
                <span class="mb-1">Amount ($)</span>
                <input class="shadow-sm focus:ring-green-400 focus:border-green-400 block w-full sm:text-sm border-gray-300 rounded-md" type="text" name="amount" id="amount" placeholder="10.00" v-model="form.amount" required>
            </label>
            <div class="flex items-center space-x-6">
                <label for="method_add">
                    <input class="h-4 w-4 text-green-400 border-gray-300 focus:ring-green-400" type="radio" name="method" id="method_add" value="add" v-model="form.method" required @change="handleMethodChange">
                    <span class="ml-3 font-medium text-gray-600">Add GST</span>
                </label>
                <label for="method_subtract">
                    <input class="h-4 w-4 text-green-400 border-gray-300 focus:ring-green-400" type="radio" name="method" id="method_subtract" value="subtract"  v-model="form.method" required @change="handleMethodChange">
                    <span class="ml-3 font-medium text-gray-600">Subtract GST</span>
                </label>
            </div>
            <button type="submit" class="inline-flex justify-center items-center px-3 py-2 border border-transparent text-sm leading-4 font-bold rounded-md shadow-sm text-white bg-green-500 hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-400">Calculate</button>
        </form>

        <ul v-if="results.total" class="flex flex-col bg-green-50 space-y-4 p-6 rounded-md font-bold text-gray-600 border border-green-400">
            <li>
                <span class="text-green-500">Subtotal:</span> {{ results.subtotal }}
            </li>
            <li>
                <span class="text-green-500">GST:</span> {{ results.gst }}
            </li>
            <li>
                <span class="text-green-500">Total:</span> {{ results.total }}
            </li>
        </ul>
    </div>
</template>

<script>
import { defineComponent, reactive } from "vue";
import useGst from 'gst-utils';

    export default defineComponent({
        setup() {
            const form = reactive({
                amount: '',
                method: 'add'
            });

            function format(value, decimals = 2) {
                const res = Number(value).toLocaleString('en', {
                    minimumFractionDigits: decimals,
                    maximumFractionDigits: decimals,
                });

                return `$${res}`;
            }

            const results = reactive({
                subtotal: '',
                gst: '',
                total: ''
            });

            function handleMethodChange() {
                if (form.amount) {
                    handleSubmit();
                }
            }


            function handleSubmit() {
                const { gst, amountInclusive, amountExclusive } = useGst(form.amount);

                switch (form.method) {
                    case 'add':
                        results.gst = format((amountInclusive - (amountExclusive + gst)));
                        results.total = format(amountInclusive);
                    break;
                    case 'subtract':
                        results.subtotal = format(amountInclusive - gst);
                        results.gst = format(gst);
                        results.total = format(amountExclusive);
                    break;
                    default:
                        break;
                }   

                results.subtotal = format(amountExclusive + gst);
            }

            return {
                form,
                handleSubmit,
                results,
                handleMethodChange,
            }
        }
    });
</script>