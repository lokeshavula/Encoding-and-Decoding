<script setup lang="ts">
import { ArrowRightLeft, Copy, RefreshCw } from 'lucide-vue-next';
import { ref } from 'vue';
import { Button } from '@/components/ui/button';
import { Textarea } from '@/components/ui/textarea';
import { Label } from '@/components/ui/label';
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group';

const inputText = ref('');
const outputText = ref('');
const mode = ref('encode');

const handleConversion = () => {
    if (mode.value === 'encode') {
        outputText.value = btoa(inputText.value);
    } else {
        try {
            outputText.value = atob(inputText.value);
        } catch (error) {
            outputText.value = 'Invalid Base64 string';
        }
    }
};

const copyToClipboard = (text: string) => {
    navigator.clipboard.writeText(text);
};

const clearAll = () => {
    inputText.value = '';
    outputText.value = '';
};
</script>

<template>
    <div class="container border-2 border-gray-200 rounded-lg mx-auto px-4 py-8">
        <div class="max-w-4xl mx-auto">
            <!-- Header -->
            <div class="mb-8">
                <h1 class="text-3xl font-bold mb-2">Base64 Text Encoder/Decoder</h1>
                <p class="text-gray-500">Configuation</p>
            </div>

            <!-- Mode Selection -->
            <div class="mb-6">
                <RadioGroup v-model="mode" class="flex gap-4">
                    <div class="flex items-center space-x-2">
                        <RadioGroupItem value="encode" id="encode" />
                        <Label for="encode">Encode</Label>
                    </div>
                    <div class="flex items-center space-x-2">
                        <RadioGroupItem value="decode" id="decode" />
                        <Label for="decode">Decode</Label>
                    </div>
                </RadioGroup>
            </div>

            <!-- Input Section -->
            <div class="mb-6">
                <Label class="mb-2 block">Input Text</Label>
                <div class="relative">
                    <Textarea v-model="inputText"
                        :placeholder="mode === 'encode' ? 'Enter text to encode' : 'Enter Base64 to decode'"
                        class="min-h-[200px] font-mono" />
                    <Button v-if="inputText" variant="ghost" size="icon" class="absolute top-2 right-2"
                        @click="() => copyToClipboard(inputText)">
                        <Copy class="h-4 w-4" />
                    </Button>
                </div>
            </div>

            <!-- Action Buttons -->
            <div class="flex gap-4 mb-6">
                <Button @click="handleConversion" class="flex items-center gap-2">
                    <ArrowRightLeft class="h-4 w-4" />
                    {{ mode === 'encode' ? 'Encode' : 'Decode' }}
                </Button>
                <Button variant="outline" @click="clearAll" class="flex items-center gap-2">
                    <RefreshCw class="h-4 w-4" />
                    Clear All
                </Button>
            </div>

            <!-- Output Section -->
            <div>
                <Label class="mb-2 block">Output</Label>
                <div class="relative">
                    <Textarea v-model="outputText" readonly class="min-h-[200px] font-mono bg-gray-50"
                        :placeholder="mode === 'encode' ? 'Encoded text will appear here' : 'Decoded text will appear here'" />
                    <Button v-if="outputText" variant="ghost" size="icon" class="absolute top-2 right-2"
                        @click="() => copyToClipboard(outputText)">
                        <Copy class="h-4 w-4" />
                    </Button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container {
    min-height: calc(100vh - 4rem);
}
</style>