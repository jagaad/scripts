#!/usr/bin/env node

import { argv } from 'node:process';
import { resolve } from 'node:path';
import { readFileSync, writeFileSync, existsSync } from 'node:fs';
import { getType } from '@jagaad/utils';

const files = argv.slice(2);

const jsonFiles = files
	.map((file) => resolve(file))
	.filter((path) => path.endsWith('.json'))
	.filter((path) => existsSync(path));

jsonFiles.forEach((path) => {
	console.log('Sorting:', path);
	const strings = JSON.parse(readFileSync(path, 'utf8'));

	if (!getType(strings, 'object')) return;

	const sortedStrings = sortObject(strings);
	const content = JSON.stringify(sortedStrings, null, 2);
	writeFileSync(path, content);
});

function sortObject(obj) {
	const entries = Object.entries(obj);

	const sortedEntries = entries
		.toSorted(([a], [b]) => a.localeCompare(b))
		.map(([k, v]) => (getType(v, 'object') ? [k, sortObject(v)] : [k, v]));

	return Object.fromEntries(sortedEntries);
}
