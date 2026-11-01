+++
title = 'testing'
date = '2026-07-06'
draft = true
+++

```ts
/// <reference types="node" />

import fs, { readFileSync as read, type Stats } from "node:fs";
import * as path from "node:path";
import data from "./data.json" assert { type: "json" };
export * from "./other";
export { foo as bar };
export default class DefaultExport {}

const enum Flags { None = 0, Read = 1, Write = 2 }
enum Color { Red = "red", Green = "green", Blue = "blue" }

namespace Geometry {
    export interface Point { x: number; y: number }
    export const origin: Point = { x: 0, y: 0 };
}

type Primitive = string | number | bigint | boolean | symbol | null | undefined;
type Pair<T = string> = readonly [T, T?];
type Keys<T> = keyof T;
type Box<T> = { value: T };
type PartialBox<T> = { [K in keyof T]?: T[K] };
type RequiredBox<T> = { [K in keyof T]-?: T[K] };
type ReadonlyBox<T> = Readonly<T>;
type Excluded = Exclude<"a" | "b" | "c", "b">;
type Extracted = Extract<string | number | boolean, number | boolean>;
type Flatten<T> = T extends readonly (infer U)[] ? U : T;
type EventName<T extends string> = `on${Capitalize<T>}`;
type Id = string & { readonly brand: unique symbol };

interface Animal { name: string; speak(): void }
interface Dog extends Animal { bark(): void }
type Callable = { (x: number): number; description: string };

declare global {
    interface Array<T> {
        first(): T | undefined;
    }
}

function identity<T>(value: T): T;
function identity(value: unknown) { return value; }

async function fetchThing(id: number): Promise<string> {
    await Promise.resolve();
    return `${id}`;
}

function* counter() {
    yield 1;
    yield* [2, 3];
    return 4;
}

const asyncArrow = async <T extends object>(value: T): Promise<T> => value;
const square = (n: number) => n * n;

class Base {
    protected value = 123;
    get number() { return this.value; }
    set number(v: number) { this.value = v; }
}

class Derived extends Base implements Dog {
    static version = "1.0";
    readonly id = crypto.randomUUID?.() ?? "id";
    #secret = true;

    constructor(public name: string, private age = 0) {
        super();
    }

    bark() {}
    speak() {}

    override get number() {
        return super.number;
    }

    async method<T extends Primitive>(value: T): Promise<T> {
        return value;
    }
}

const numbers = [1, 2, 3, ...[4, 5]];
const tuple = [1, "two"] as const;
const object = {
    a: 1,
    b: "text",
    ["dynamic"]: true,
    nested: { x: 1 },
    method() {},
    async asyncMethod() {},
    *generator() { yield 1; },
};

const { a, nested: { x }, ...rest } = object;
const [first, , third = 3, ...others] = numbers;

let value: unknown = Math.random() > 0.5 ? 42 : "hello";

if (typeof value === "string") {
    value.toUpperCase();
} else if (value instanceof Derived) {
    value.bark();
} else {
    value satisfies unknown;
}

switch (a) {
    case 1:
        break;
    default:
        break;
}

for (const n of numbers) console.log(n);
for (const k in object) console.log(k);

let i = 0;
while (i < 3) i++;
do i--; while (i > 0);

try {
    throw new Error("boom");
} catch (err) {
    console.error(err);
} finally {
    console.log("done");
}

const map = new Map<string, number>();
const set = new Set<number>();

const promise = Promise.resolve(123)
    .then(n => n * 2)
    .catch(() => 0)
    .finally(() => {});

const regex = /^(?<name>[a-z_]\w*)$/giu;
const bigint = 123_456n;
const hex = 0xff;
const oct = 0o755;
const bin = 0b101010;
const sci = 6.022e23;
const nan = NaN;
const inf = Infinity;

const template = `
hello ${name}
sum = ${1 + 2}
`;

const optional = object?.nested?.x ?? 0;
const indexed = object["dynamic"];
const deleted = delete (object as any).missing;
const exists = "a" in object;
const instance = new Derived("dog") instanceof Base;

const cast1 = value as string;
const cast2 = <number>42;

const frozen = {
    mode: "fast",
    retry: 3,
} as const;

const config = {
    host: "localhost",
    port: 8080,
} satisfies {
    host: string;
    port: number;
};

function takesCallback(cb: (x: number) => void) {
    cb(123);
}

takesCallback(n => console.log(n));

/**
 * Adds two numbers.
 * @template T
 * @param {T} a
 * @param {T} b
 * @returns {T}
 * @example
 * add(1, 2)
 * @see https://example.com
 */
function add<T extends number>(a: T, b: T): T {
    return (a + b) as T;
}
```
