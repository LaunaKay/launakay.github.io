---
layout: post
title: Simple D3 Component
---
# WRITING STILL IN PROGRESS. READ AT YOUR OWN RISK

```
import { Component, ElementRef, OnInit } from '@angular/core';
import * as D3 from 'd3';

@Component({
    selector: 'counter',
    template: require('./counter.component.html')
})
export class CounterComponent {

    constructor(public _el: ElementRef) { };

    public currentCount = 0;
    chartArea;
    chartSelection;
    update;
    enter;

    public incrementCounter() {
        this.currentCount++;
    }

    public scores = [
        { 'name': 'Alice', 'score': 96 },
        { 'name': 'Billy', 'score': 83 },
        { 'name': 'Cindy', 'score': 93 },
        { 'name': 'David', 'score': 96 },
        { 'name': 'Emily', 'score': 88 }
    ];

    ngOnInit() {
        this.chartArea = D3.select(this._el.nativeElement);
        this.chartSelection = this.chartArea.select('.chart')
        console.log('chartSelection', this.chartSelection.nodes());
        this.update = this.chartSelection
            .selectAll('div')
            .data(this.scores, function (d) {
                return d ? d.name : this.innerText;
            })
            .style('color', 'blue');

        this.enter = this.update.enter()
            .append('div')
            .text(function (d) {
                return d.name;
            })
            .style('color', 'green');

        this.update.exit().remove();

        this.update.merge(this.enter)
            .style('width', d => d.score + 'px')
            .style('height', '50px')
            .style('background', 'lightgreen')
            .style('border', '1px solid black')
    }
}
