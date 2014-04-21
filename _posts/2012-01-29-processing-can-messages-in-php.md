---
layout: post
title:  "Processing CAN messages in PHP"
date:   2012-01-29 06:44:47
author: Michael
---
he automotive industry makes extensive use of a standard called CAN bus, a Controller Area Network which allows the various components of a vehicle to talk to each other without the need for a host. Typically, messages sent on the CAN bus are short 16 character hexadecimal words, with an identifier associated with them. To get from these 16 character words to meaningful information you need two things: a CAN specification provided by the relevant component manufacturer, and a way to process the information and decode it as per the specification.

Recently, I've been doing some work which has involved verifying the information that has been extracted from the CAN bus is correct, and that we are interpreting the CAN specification correctly. This has proved difficult, as while I can see where the specification hasn't been correctly translated into the application which processes it, it isn't easy to see (when it has been translated correctly) where the incorrect values are coming from. To make things easier, and to try and further my knowledge of the CAN bus, I have written a PHP based CAN processing tool. This allows me to perform the translations and calculations myself and see why certain pieces of information have been translated incorrectly.

As a seperate project I'm working with a group of engineers to develop a small autonomous robot, which will use a CAN bus to keep itself aware of its location and what its various components are doing. With this PHP tool, I will be able to quickly process what it is doing and display this in a web browser without the need for some middleware application.

The CAN processing tool is relatively simple. You provide an XML based specification detailing which type of CAN bus messages can be translated into which useful pieces of information, detailing which bytes and bits of the hexadecimal word contain the information, as well as which scaling, multiplication and data type conversion (e.g. convert two a short by applying a two's compliment) factors need to be applied to get the correct output.

## Get the code
The code is available on [GitHub](https://github.com/mkpeacock/PHP-CAN-Processor).

## How to use it

There are two ways to use the application, you can either provide an XML specification to the application, pass a message and let it do the work, or you can manually tell the application which messages need to be translated in which ways.

### Providing a specification
The easiest way to use this system is to provide an XML based "specification" of the different CAN message types you wish to decode, and the pieces of information from within.

```
<?xml version="1.0" encoding="UTF-8">
<canspec>
    <canID id="ABC" type="multiplex" multiplexByte="0" namePrefix="" nameSuffix="_Piston%1$d">
        <engineeringUnit type="multi-byte">
            <name>Piston_position</name>
            <unit>mm</unit>
            <mostSignificantByte>2</mostSignificantByte>
            <leastSignificantByte>3</leastSignificantByte>
            <dataType<null</dataType>
            <multiplier>0.01</multiplier>
            <offset>0</offset>
        </engineeringUnit>
    </canID>
</canspec>
```

You can then use a factory to build the keys, the decoders and turn that into a collection of decoders.

    $factory = new CentralApps\CAN\Decoder_Factory();
    $spec = $factory->makeDecoderFromXMLFile('sample_can_specification.xml');

Passing a message to the collection of decoders will find the decoder for that can message type, and perform the decoding logic from within.

    $message = new CentralApps\CAN\Message( 'ABC', '0000000000000000' );
    $units = $spec->decode( $message );
    foreach ($units as $unit) {
        echo $unit;
    }

### Manual

    $message = new CentralApps\CAN\Message( 'ABC', '0000000000000000' );

    $multi_byte_key = new CentralApps\CAN\Decoder_Keys_MultiByte();
    $multi_byte_key->setCanID('ABC');
    $multi_byte_key->setName('Piston_position');
    $multi_byte_key->setUnit("mm");
    $multi_byte_key->setMultiplier(0.01);
    $multi_byte_key->setOffset(0);
    $multi_byte_key->setMostSignificantByte(2);
    $multi_byte_key->setLeastSignificantByte(3);
    $multi_byte_key->setDataLength(16);
    $multi_byte_key->setDataType('short');

    $decoder = new CentralApps\CAN\Decoder_Multiplex();
    $decoder->setMultiplexByte(0);
    $decoder->setNameSuffix('_Pison%1$d');
    $decoder->add($multi_byte_key);
    $units = $decoder->decode($message);
    foreach ($units as $unit) {
        echo $unit;
    }

## Other notes

This tool makes use of the PSR-0 autoloading standard, so if you use an autoloader compatible with this you should just be able to drop the code in. If you find you need to support another data type conversion, simply create a new class in the /src/CentralApps/CAN/Decoder/DataTypes folder which implements the Decoder_DataTypes_Interface. If you need to decode information differently, create a new class in /src/CentralApps/CAN/Decoder/Keys which extends the Decoder_Core class. You will need to update the Decider_Factory to correctly build decoder keys of that type.
