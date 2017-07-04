var utils = {
	getType: function(content) {
		return Object.prototype.toString.call(content).match(/\[object (.*)\]/)[1];
	},
	isType: function(content, type) {
		return this.getType(content).toLowerCase() === type.toLowerCase();
	}

};

function Iterator(content, cfg) {
	this.content = content;
	this.length = 0;
	this.count = 0;
	this.position = 0;
	this.direction = (cfg && cfg.direction) || 0;
	this.init(cfg);
}
Iterator.prototype = {
	next: function() {
		if (!this.hasNext()) {
			return null;
		}
		this.count++;
		if (this.direction === 0) {
			return this.content[this.position++];
		} else {
			return this.content[this.position--];
		}
		
	},	
	rewind: function(index) {
		if (index  < this.length && index >= 0) {
			this.position = index;
			this.count = this.direction === 0 ? index : this.length - index - 1;
		}
	},
	hasNext: function() {
		return this.count < this.length;
	},
	init: function(cfg) {
		if (cfg && cfg.contentHandler && utils.isType(cfg.contentHandler, 'function')) {
			var result = cfg.contentHandler.call(this);
			if (utils.isType(result, 'Array')) {
				this.content = result;
			} else {
				throw new Error('arguments contentHandler of Iterator Error');
				return;
			}
		} else {
			var type = utils.getType(this.content);
			switch(type) {
				case 'String': 
								this.content = this.content.split('');
								break;
				case 'Array': 
								this.content = this.content.concat();
								break;
				case 'Object': 
							var arr = [];
							var keys = Object.keys(this.content);
							for (var i = 0; i< keys.length; i++) {
								var obj = {};
								obj[keys[i]] = this.content[keys[i]];
								arr.push(obj);
							}
							this.content = arr;
							break;
			}
		}
		
		this.length = this.content.length;
		this.position = this.direction === 0 ? 0 : this.length - 1;
	}
}
