# LuaEventMediator
Event mediator for Cocos2d-x.

## First and Foremost

Cocos2d-x has provided a relatively perfect object-oriented programming module, there is no need to redefine a "Class" module. 
So this case runs directly in Cocos2d-x.

## Typical Example

    local EventMediator = require "EventMediator"
    cc.exports.eventMediator = EventMediator.new()
    
    -- some dummy A class
    local A = class("A")

    function A:ctor()
        eventMediator:listen("EventName", function(...)
            print(...)
        end, 0)
    end

    -- B class
    local B = class("B")

    function B:ctor()
        eventMediator:publish("EventName", 123, 'hello A')
    end
    
    -- test
    A.new()
    B.new()
    -- print: 123   hello A

Super simple!

## Documentation

function EventObj:bindHost()


    local NodeA = class("NodeA", CocosNode)
      
    function NodeA:ctor()
        NodeA.super.ctor(self)

        self.listeners = {
            eventMediator:listener("EventName",function() end):bindHost(self)
        }
    end

    function NodeA:onExit()
        NodeA.super.onExit(self)

        -- Once bindHost, you can skip the "removeListener things" like below:
        -- for _,v in pairs(self.listeners) do
        --      eventMediator:removeListener(v)                
        -- end
    end

function EventMediator.inject()


    local NodeA = class("NodeA", CocosNode)
      
    function NodeA:ctor()
        NodeA.super.ctor(self)
        
        -- to make a node "eventable"
        EventMediator.inject(self)

        self:initListeners()
    end

    function NodeA:initListeners()
        self:addListener("SomeEvent", function() end) --"bindHost" is included.
    end

    -- Is it useful? ^_^
